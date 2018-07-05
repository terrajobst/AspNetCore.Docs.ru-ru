---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Основы ASP.NET MVC 4 | Документация Майкрософт
author: rick-anderson
description: Этот практический семинар основан на MVC (Model View Controller) Music Store, учебного приложения, которые вводятся и описываются способы использования ASP.NET MV пошаговые...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386579"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="13d1d-103">Основы ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="13d1d-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="13d1d-104">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="13d1d-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="13d1d-105">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="13d1d-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="13d1d-106">Это Практическое лабораторное занятие основано на MVC (Model View Controller) Music Store, учебного приложения, которое вводятся и описываются пошаговые как использовать ASP.NET MVC и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="13d1d-107">В лаборатории. Вы узнаете, простоты еще питания друг с другом с помощью этих технологий.</span><span class="sxs-lookup"><span data-stu-id="13d1d-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="13d1d-108">Начнем с простого приложения и будет постройте его, пока у вас есть полнофункциональное приложение ASP.NET MVC 4 Web.</span><span class="sxs-lookup"><span data-stu-id="13d1d-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="13d1d-109">Это лабораторное занятие работает с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="13d1d-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="13d1d-110">Если вы хотите просмотреть версии ASP.NET MVC 3 учебного приложения, его можно найти в [MVC-музыка-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="13d1d-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="13d1d-111">Это Практическое лабораторное занятие предполагает, что разработчик имеет опыт работы в веб-технологии разработки, таких как HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="13d1d-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="13d1d-112">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="13d1d-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="13d1d-113">Проект, относящиеся к этой лаборатории доступен в [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="13d1d-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="13d1d-114">Приложение Music Store</span><span class="sxs-lookup"><span data-stu-id="13d1d-114">The Music Store application</span></span>

<span data-ttu-id="13d1d-115">Веб-приложение Music Store, который будет построен на протяжении всего этого лабораторного состоит из трех основных частей: покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="13d1d-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="13d1d-116">Посетители смогут Обзор альбомов по жанру, добавьте альбомов в корзину, просмотрите свой выбор и наконец перейдите к извлечение, чтобы войти в систему и завершить создание заказа.</span><span class="sxs-lookup"><span data-stu-id="13d1d-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="13d1d-117">Кроме того Администраторы хранилища будут иметь возможность управлять доступных альбомов, а также их основные свойства.</span><span class="sxs-lookup"><span data-stu-id="13d1d-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="13d1d-118">![Экраны Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "экраны Music Store")</span><span class="sxs-lookup"><span data-stu-id="13d1d-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="13d1d-119">*Экраны Music Store*</span><span class="sxs-lookup"><span data-stu-id="13d1d-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="13d1d-120">Основы ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="13d1d-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="13d1d-121">Приложение Music Store будет создано с помощью **Model View Controller (MVC)**, архитектурный шаблон, который разделяет приложение на три основных компонента:</span><span class="sxs-lookup"><span data-stu-id="13d1d-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="13d1d-122">**Модели**: объекты моделей являются частями приложения, реализующими доменную логику.</span><span class="sxs-lookup"><span data-stu-id="13d1d-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="13d1d-123">Часто объекты модели также получить и сохраняют состояние модели в базе данных.</span><span class="sxs-lookup"><span data-stu-id="13d1d-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="13d1d-124">**Представления:** представления служат для отображения приложения пользовательского интерфейса (UI).</span><span class="sxs-lookup"><span data-stu-id="13d1d-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="13d1d-125">Как правило этот пользовательский Интерфейс создается на основе модели данных.</span><span class="sxs-lookup"><span data-stu-id="13d1d-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="13d1d-126">Примером может служить представление редактирования альбомов, содержит текстовые поля и стрелку раскрывающегося списка, на основании текущего состояния объекта альбома.</span><span class="sxs-lookup"><span data-stu-id="13d1d-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="13d1d-127">**Контроллеры:** контроллеров служат для управления взаимодействием с пользователем, управлять модели и выбора представления для отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="13d1d-128">В приложении MVC представление служит только для отображения информации. Обработку введенных данных, формирование ответа и взаимодействие с пользователем обеспечивает контроллер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="13d1d-129">Шаблон MVC позволяет создавать приложения, различные аспекты приложения (логика ввода, бизнес-логика и логика пользовательского интерфейса), при этом слабые взаимозависимости между этими элементами.</span><span class="sxs-lookup"><span data-stu-id="13d1d-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="13d1d-130">Это разделение позволяет справляться с трудностями при создании приложения, так как это позволяет вам сосредоточиться на один из аспектов реализации за раз.</span><span class="sxs-lookup"><span data-stu-id="13d1d-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="13d1d-131">Кроме того шаблон MVC позволяет легко тестировать приложения, также поощряя к использованию разработки через тестирование (TDD) для создания приложений.</span><span class="sxs-lookup"><span data-stu-id="13d1d-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="13d1d-132">**ASP.NET MVC** framework представляет собой альтернативу шаблону веб-форм ASP.NET для создания приложений веб-узла ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="13d1d-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="13d1d-133">**ASP.NET MVC** framework — это платформа представления легковесной, (как и в случае с приложениями, веб форм) интегрируется с существующими функциями ASP.NET, такие как главные страницы и на основе членства Проверка подлинности, чтобы получить все возможности платформы .NET core.</span><span class="sxs-lookup"><span data-stu-id="13d1d-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="13d1d-134">Это полезно, если вы уже знакомы с веб-форм ASP.NET, так как все библиотеки, которые уже используют в ASP.NET MVC 4, также доступны.</span><span class="sxs-lookup"><span data-stu-id="13d1d-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="13d1d-135">Кроме того слабой связи между три основных компонента приложения MVC также облегчает параллельную разработку.</span><span class="sxs-lookup"><span data-stu-id="13d1d-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="13d1d-136">К примеру один разработчик может создавать представление, второй разработчик может работать на логике контроллера и третий разработчик может сосредоточиться на бизнес-логику в модели.</span><span class="sxs-lookup"><span data-stu-id="13d1d-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="13d1d-137">Цели</span><span class="sxs-lookup"><span data-stu-id="13d1d-137">Objectives</span></span>

<span data-ttu-id="13d1d-138">В этом Практическое лабораторное занятие, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="13d1d-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="13d1d-139">Создание приложения ASP.NET MVC с нуля, в зависимости от руководства по этому приложению Music Store</span><span class="sxs-lookup"><span data-stu-id="13d1d-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="13d1d-140">Добавление контроллеров для обработки URL-адреса на домашнюю страницу сайта, а также для просмотра его главные функциональные возможности</span><span class="sxs-lookup"><span data-stu-id="13d1d-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="13d1d-141">Добавление представления для настройки содержимого, отображаемого вместе с его стиль</span><span class="sxs-lookup"><span data-stu-id="13d1d-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="13d1d-142">Добавление классов модели для хранения и управления логику данных и домена</span><span class="sxs-lookup"><span data-stu-id="13d1d-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="13d1d-143">Используйте шаблон View Model для передачи данных из действий контроллера в представление шаблоны</span><span class="sxs-lookup"><span data-stu-id="13d1d-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="13d1d-144">Изучите новый шаблон ASP.NET MVC 4 для Интернет-приложений</span><span class="sxs-lookup"><span data-stu-id="13d1d-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="13d1d-145">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="13d1d-145">Prerequisites</span></span>

<span data-ttu-id="13d1d-146">Необходимо иметь следующие элементы во укомплектовать данную лабораторию:</span><span class="sxs-lookup"><span data-stu-id="13d1d-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="13d1d-147">[Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (чтение [приложение А](#AppendixA) инструкции по его установке)</span><span class="sxs-lookup"><span data-stu-id="13d1d-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="13d1d-148">Установка</span><span class="sxs-lookup"><span data-stu-id="13d1d-148">Setup</span></span>

<span data-ttu-id="13d1d-149">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="13d1d-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="13d1d-150">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="13d1d-151">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="13d1d-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="13d1d-152">Если вы не знакомы с фрагменты кода Visual Studio и хотите научиться использовать их, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="13d1d-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="13d1d-153">Упражнения</span><span class="sxs-lookup"><span data-stu-id="13d1d-153">Exercises</span></span>

<span data-ttu-id="13d1d-154">Это Практическое лабораторное занятие включает по следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="13d1d-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="13d1d-155">Упражнение 1: Создание проекта веб-приложения MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d1d-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="13d1d-156">Упражнение 2: Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="13d1d-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="13d1d-157">Упражнение 3: Передача параметров к контроллеру</span><span class="sxs-lookup"><span data-stu-id="13d1d-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="13d1d-158">Упражнение 4: Создание представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="13d1d-159">Упражнение 5: Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="13d1d-160">Упражнение 6: Использование параметров в представлении</span><span class="sxs-lookup"><span data-stu-id="13d1d-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="13d1d-161">Упражнение 7: Краткая информация о новый шаблон ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="13d1d-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="13d1d-162">Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="13d1d-163">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.</span><span class="sxs-lookup"><span data-stu-id="13d1d-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="13d1d-164">Предполагаемое время для выполнения этого лабораторного: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="13d1d-165">Упражнение 1: Создание проекта веб-приложения MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d1d-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="13d1d-166">В этом упражнении вы узнаете, как создать приложение ASP.NET MVC в Visual Studio 2012 Express для Web, а также их организация в главную папку.</span><span class="sxs-lookup"><span data-stu-id="13d1d-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="13d1d-167">Кроме того вы узнаете, как добавить новый контроллер и сделать его отобразить простую строку на домашней странице приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="13d1d-168">Задача 1 - Создание проекта веб-приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d1d-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="13d1d-169">В этой задаче вы создадите пустого проекта приложения ASP.NET MVC с помощью шаблона MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="13d1d-170">Запуск **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="13d1d-171">В меню **Файл** выберите пункт **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="13d1d-172">В **новый проект** диалогового окна выберите **веб-приложение ASP.NET MVC 4** тип, расположенный в разделе проекта **Visual C#,** **Web** шаблона список.</span><span class="sxs-lookup"><span data-stu-id="13d1d-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="13d1d-173">Изменение **имя** для *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="13d1d-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="13d1d-174">Задайте расположение решения в новое **начать** папку в папке-источнике в этом упражнении, например **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-ХОЛЬ-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="13d1d-175">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-175">Click **OK**.</span></span>

    <span data-ttu-id="13d1d-176">![Создание диалогового окна "новый проект"](aspnet-mvc-4-fundamentals/_static/image2.png "создать диалоговое окно \"новый проект\"")</span><span class="sxs-lookup"><span data-stu-id="13d1d-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="13d1d-177">*Создание диалогового окна "новый проект"*</span><span class="sxs-lookup"><span data-stu-id="13d1d-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="13d1d-178">В **создания проекта ASP.NET MVC 4** диалогового окна выберите **основные** шаблона и убедитесь, что **обработчик представлений** выбрано **Razor**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="13d1d-179">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-179">Click **OK**.</span></span>

    <span data-ttu-id="13d1d-180">![Диалоговое окно "новый проект ASP.NET MVC 4"](aspnet-mvc-4-fundamentals/_static/image3.png "диалоговое окно \"новый проект ASP.NET MVC 4\"")</span><span class="sxs-lookup"><span data-stu-id="13d1d-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="13d1d-181">*Диалоговое окно "новый проект ASP.NET MVC 4"*</span><span class="sxs-lookup"><span data-stu-id="13d1d-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="13d1d-182">Задача 2 - изучение структуры решения</span><span class="sxs-lookup"><span data-stu-id="13d1d-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="13d1d-183">Платформа ASP.NET MVC включает в себя шаблон проекта Visual Studio, который поможет вам создать веб-приложений, которые поддерживают шаблон MVC.</span><span class="sxs-lookup"><span data-stu-id="13d1d-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="13d1d-184">Этот шаблон создает новое приложение веб-ASP.NET MVC с необходимые папки, шаблоны элементов и записи файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="13d1d-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="13d1d-185">В этой задаче вы исследуете структуру решения, чтобы понять, элементы, которые участвуют и их связи.</span><span class="sxs-lookup"><span data-stu-id="13d1d-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="13d1d-186">Поскольку платформа ASP.NET MVC по умолчанию использует следующие папки включаются в все приложения ASP.NET MVC &quot;соглашение относительно настройки&quot; подход и делает некоторые предположения по умолчанию зависимости от папки именования соглашения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="13d1d-187">После создания проекта Просмотрите структуру папок, которая будет создана в обозревателе решений правой стороны:</span><span class="sxs-lookup"><span data-stu-id="13d1d-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="13d1d-188">![Структура папки MVC ASP.NET в обозревателе решений](aspnet-mvc-4-fundamentals/_static/image4.png "структура папки MVC ASP.NET в обозревателе решений")</span><span class="sxs-lookup"><span data-stu-id="13d1d-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="13d1d-189">*Структура папки MVC ASP.NET в обозревателе решений*</span><span class="sxs-lookup"><span data-stu-id="13d1d-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="13d1d-190">**Контроллеры**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-190">**Controllers**.</span></span> <span data-ttu-id="13d1d-191">Эта папка будет содержать классы контроллера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="13d1d-192">В приложении на базе MVC контроллеры отвечают за обработку взаимодействия с конечным пользователем, работы с моделью и в конечном счете Выбор представления для отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="13d1d-193">MVC требует имена всех контроллеров с присвоением &quot;контроллера&quot;-например, HomeController, LoginController или ProductController.</span><span class="sxs-lookup"><span data-stu-id="13d1d-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="13d1d-194">**Модели**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-194">**Models**.</span></span> <span data-ttu-id="13d1d-195">Эта папка предоставляется для классов, которые представляют модель приложения для веб-приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="13d1d-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="13d1d-196">Это обычно содержит код, который определяет объекты и логику для взаимодействия с хранилищем данных.</span><span class="sxs-lookup"><span data-stu-id="13d1d-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="13d1d-197">Как правило сами объекты модели будет в отдельных библиотеках классов.</span><span class="sxs-lookup"><span data-stu-id="13d1d-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="13d1d-198">Тем не менее при создании нового приложения, может включить классы и переместить их в отдельные библиотеки на более позднем этапе цикла разработки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="13d1d-199">**Представления**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-199">**Views**.</span></span> <span data-ttu-id="13d1d-200">Эта папка — рекомендуемое расположение для представлений, компоненты, ответственные за отображение пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="13d1d-201">Представления используют файлы .aspx, .ascx, cshtml и .master, помимо других файлов, которые связаны с отображением представлений.</span><span class="sxs-lookup"><span data-stu-id="13d1d-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="13d1d-202">Папка Views содержит папки для каждого контроллера; папка называется с префиксом имени контроллера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="13d1d-203">Например, если у вас есть контроллер с именем **HomeController**, папку Views будет содержать папку с именем Home.</span><span class="sxs-lookup"><span data-stu-id="13d1d-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="13d1d-204">По умолчанию при загрузке представления платформой ASP.NET MVC выполняется поиск ASPX-файл с именем требуемого представления в папке Views\controllerName (**представлений [Имя_контроллера] [действие] .aspx**) или (**представлений [Имя_контроллера] [Действие] .cshtml**) для представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="13d1d-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13d1d-205">В дополнение к перечисленным выше папкам веб-приложение MVC использует **Global.asax** файла для установки глобальной маршрутизации URL-адрес по умолчанию и использует **Web.config** файл для настройки приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="13d1d-206">Задача 3 - Добавление HomeController</span><span class="sxs-lookup"><span data-stu-id="13d1d-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="13d1d-207">В приложениях ASP.NET, не использующих платформу MVC взаимодействие с пользователем организована вокруг страниц, а также создания и обработки событий из этих страниц.</span><span class="sxs-lookup"><span data-stu-id="13d1d-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="13d1d-208">Напротив взаимодействие пользователей с приложениями ASP.NET MVC основаны на контроллерах и методах их действия.</span><span class="sxs-lookup"><span data-stu-id="13d1d-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="13d1d-209">С другой стороны платформа ASP.NET MVC сопоставляет URL-адреса с классами, называются контроллеров.</span><span class="sxs-lookup"><span data-stu-id="13d1d-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="13d1d-210">Контроллеры обрабатывают входящие запросы, вводимые пользователями данные и взаимодействий, реализуют необходимую логику приложений и определения ответа для отправки обратно клиенту (отображения HTML-кода, загрузите файл, перенаправления на другой URL-адрес, и т.п.).</span><span class="sxs-lookup"><span data-stu-id="13d1d-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="13d1d-211">В случае отображения HTML, класс контроллера обычно вызывает отдельный компонент представления для создания разметки HTML для запроса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="13d1d-212">В приложении MVC представление служит только для отображения информации. Обработку введенных данных, формирование ответа и взаимодействие с пользователем обеспечивает контроллер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="13d1d-213">В этой задаче вы добавите класс контроллера, который будет обрабатывать URL-адреса на домашнюю страницу сайта Music Store.</span><span class="sxs-lookup"><span data-stu-id="13d1d-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="13d1d-214">Щелкните правой кнопкой мыши **контроллеров** папку в обозревателе решений, выберите **добавить** и затем **контроллера** команды:</span><span class="sxs-lookup"><span data-stu-id="13d1d-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="13d1d-215">![Добавление команды контроллера](aspnet-mvc-4-fundamentals/_static/image5.png "Добавление команды контроллера")</span><span class="sxs-lookup"><span data-stu-id="13d1d-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="13d1d-216">*Добавьте команду контроллера*</span><span class="sxs-lookup"><span data-stu-id="13d1d-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="13d1d-217">**Добавление контроллера** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="13d1d-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="13d1d-218">Назовите контроллер *HomeController* и нажмите клавишу **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="13d1d-219">![Диалоговое окно добавления контроллера](aspnet-mvc-4-fundamentals/_static/image6.png "диалоговое окно добавления контроллера")</span><span class="sxs-lookup"><span data-stu-id="13d1d-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="13d1d-220">*Диалоговое окно добавления контроллера*</span><span class="sxs-lookup"><span data-stu-id="13d1d-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="13d1d-221">Файл **HomeController.cs** создается в **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="13d1d-222">Чтобы получить **HomeController** возвращают строку на его действие индекса, замените **индекс** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="13d1d-223">(Code Snippet - *основы ASP.NET MVC 4 - сервера Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="13d1d-224">Задача 4 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="13d1d-225">В этой задаче будет опробовать приложение в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="13d1d-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="13d1d-226">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="13d1d-227">Проект будет скомпилирован и запускает локальный веб-сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="13d1d-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="13d1d-228">Локальный веб-сервер IIS автоматически откроет в веб-браузере URL-адрес веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="13d1d-229">![Приложения, работающего в веб-браузере](aspnet-mvc-4-fundamentals/_static/image7.png "приложения, работающего в веб-браузере")</span><span class="sxs-lookup"><span data-stu-id="13d1d-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="13d1d-230">*Приложения, работающего в веб-браузере*</span><span class="sxs-lookup"><span data-stu-id="13d1d-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-231">Сайт будет работать в локальном веб-сервере IIS в случайный бесплатный номер порта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="13d1d-232">В приведенном выше рисунке узел работает в `http://localhost:50103/`, поэтому он использует порт 50103.</span><span class="sxs-lookup"><span data-stu-id="13d1d-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="13d1d-233">Ваш номер порта может меняться.</span><span class="sxs-lookup"><span data-stu-id="13d1d-233">Your port number may vary.</span></span>
2. <span data-ttu-id="13d1d-234">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="13d1d-235">Упражнение 2: Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="13d1d-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="13d1d-236">В этом упражнении вы узнаете, как контроллер для реализации простой функциональности приложение Music Store.</span><span class="sxs-lookup"><span data-stu-id="13d1d-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="13d1d-237">Этот контроллер будет определяют методы действий для обработки каждого из следующих определенные запросы:</span><span class="sxs-lookup"><span data-stu-id="13d1d-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="13d1d-238">Страницы список жанров музыку в Music Store</span><span class="sxs-lookup"><span data-stu-id="13d1d-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="13d1d-239">Страница обзора, в котором перечислены все музыкальные альбомы для определенного жанра</span><span class="sxs-lookup"><span data-stu-id="13d1d-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="13d1d-240">Сведения страницы, которая отображает сведения об определенных музыкальных альбомов</span><span class="sxs-lookup"><span data-stu-id="13d1d-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="13d1d-241">В рамках этого упражнения эти действия будут просто возвращают строку к этому моменту.</span><span class="sxs-lookup"><span data-stu-id="13d1d-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="13d1d-242">Задача 1 - Добавление нового класса StoreController</span><span class="sxs-lookup"><span data-stu-id="13d1d-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="13d1d-243">В этой задаче вы добавите новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="13d1d-244">Если это еще не открыто, запустите **VS Express for Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="13d1d-245">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="13d1d-246">В диалоговом окне Открытие проекта, перейдите к **Source\Ex02 CreatingAController\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="13d1d-247">Кроме того вы можете продолжить с решением, полученный после выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="13d1d-248">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="13d1d-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13d1d-249">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13d1d-250">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="13d1d-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13d1d-251">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13d1d-252">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13d1d-253">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13d1d-254">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="13d1d-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="13d1d-255">Добавите новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-255">Add the new controller.</span></span> <span data-ttu-id="13d1d-256">Чтобы сделать это, щелкните правой кнопкой мыши **контроллеров** папку в обозревателе решений, выберите **добавить** и затем **контроллера** команды.</span><span class="sxs-lookup"><span data-stu-id="13d1d-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="13d1d-257">Изменение **имя контроллера** для *StoreController*и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="13d1d-258">![Диалоговое окно добавления контроллера](aspnet-mvc-4-fundamentals/_static/image8.png "диалоговое окно добавления контроллера")</span><span class="sxs-lookup"><span data-stu-id="13d1d-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="13d1d-259">*Диалоговое окно добавления контроллера*</span><span class="sxs-lookup"><span data-stu-id="13d1d-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="13d1d-260">Задача 2 - изменение StoreController действия</span><span class="sxs-lookup"><span data-stu-id="13d1d-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="13d1d-261">В этой задаче вы измените методы контроллера, которые называются **действия**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="13d1d-262">Операции отвечают за обработку запросов URL-адрес и определения содержимого, которое будет отправляться обратно в браузер или пользователь, вызвавший URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="13d1d-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="13d1d-263">**StoreController** класс уже имеет **индекс** метод.</span><span class="sxs-lookup"><span data-stu-id="13d1d-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="13d1d-264">Он понадобится позже в этом лабораторном занятии реализация страницы, в которой перечислены все жанры музыкального магазина.</span><span class="sxs-lookup"><span data-stu-id="13d1d-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="13d1d-265">Пока просто замените **индекс** метод следующим кодом, которое будет возвращать строку &quot;привет от Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="13d1d-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="13d1d-266">(Code Snippet - *основы ASP.NET MVC 4 — индекс StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="13d1d-267">Добавить **Обзор** и **сведения** методы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="13d1d-268">Чтобы сделать это, добавьте следующий код, чтобы **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="13d1d-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="13d1d-269">(Code Snippet - *основы ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="13d1d-270">Задача 3 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="13d1d-271">В этой задаче будет опробовать приложение в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="13d1d-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="13d1d-272">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-273">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="13d1d-274">Измените URL-адрес для проверки реализации каждого действия.</span><span class="sxs-lookup"><span data-stu-id="13d1d-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="13d1d-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-275">**/Store**.</span></span> <span data-ttu-id="13d1d-276">Вы увидите  **&quot;привет от Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="13d1d-277">**/ Store/обзора**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-277">**/Store/Browse**.</span></span> <span data-ttu-id="13d1d-278">Вы увидите  **&quot;привет от Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="13d1d-279">**/ Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-279">**/Store/Details**.</span></span> <span data-ttu-id="13d1d-280">Вы увидите  **&quot;привет от Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="13d1d-281">![Просмотр StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "просмотра StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="13d1d-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="13d1d-282">*Просмотр /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="13d1d-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="13d1d-283">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="13d1d-284">Упражнение 3: Передача параметров к контроллеру</span><span class="sxs-lookup"><span data-stu-id="13d1d-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="13d1d-285">До сих пор вы возвращение константных строк с контроллеров.</span><span class="sxs-lookup"><span data-stu-id="13d1d-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="13d1d-286">В этом упражнении вы узнаете, как передавать параметры к контроллеру, используя URL-адрес и строки запроса, а затем выполнить метод действия отвечает с текстом в браузер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="13d1d-287">Задача 1 - Добавление параметра жанр StoreController</span><span class="sxs-lookup"><span data-stu-id="13d1d-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="13d1d-288">В этой задаче вы воспользуетесь **querystring** для отправки параметров **Обзор** метода действия в **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="13d1d-289">Если это еще не открыто, запустите **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="13d1d-290">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="13d1d-291">В диалоговом окне Открытие проекта, перейдите к **Source\Ex03 PassingParametersToAController\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="13d1d-292">Кроме того вы можете продолжить с решением, полученный после выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="13d1d-293">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="13d1d-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13d1d-294">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13d1d-295">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="13d1d-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13d1d-296">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13d1d-297">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13d1d-298">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13d1d-299">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="13d1d-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="13d1d-300">Откройте **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-300">Open **StoreController** class.</span></span> <span data-ttu-id="13d1d-301">Для этого в **обозревателе решений**, разверните **контроллеров** папку и дважды щелкните **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="13d1d-302">Изменение **Обзор** метод, добавление параметра строки запроса для определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="13d1d-303">ASP.NET MVC будет автоматически передавать любой строки запроса или отправки формы параметра с именами **жанр** этот метод действия, при вызове.</span><span class="sxs-lookup"><span data-stu-id="13d1d-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="13d1d-304">Чтобы сделать это, замените **Обзор** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="13d1d-305">(Code Snippet - *основы ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="13d1d-306">При использовании **HttpUtility.HtmlEncode** служебный метод, чтобы запрещает включение Javascript в представление со ссылкой как   **/Store/обзора? Жанр =&lt;сценарий&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="13d1d-307">Подробные пояснения см. на [в этой статье msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="13d1d-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="13d1d-308">Задача 2 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="13d1d-309">В этой задаче будет попробовать приложение в веб-браузер и использовать **жанр** параметра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="13d1d-310">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-311">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="13d1d-312">Изменить URL-адрес   */Store/Обзор? Жанр = Disco* чтобы убедиться, что действие получает параметр жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="13d1d-313">![Просмотр StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "просмотра StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="13d1d-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="13d1d-314">*Просмотр /Store/Browse? Жанр = Disco*</span><span class="sxs-lookup"><span data-stu-id="13d1d-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="13d1d-315">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="13d1d-316">Задача 3 - Добавление параметр идентификатора, URL-адрес</span><span class="sxs-lookup"><span data-stu-id="13d1d-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="13d1d-317">В этой задаче вы воспользуетесь **URL-адрес** для передачи **идентификатор** параметр **сведения** метод действия **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="13d1d-318">ASP.NET MVC по умолчанию используется соглашение о маршрутизации обрабатывать сегмента URL-адреса после имени метода действия, как параметр с именем **идентификатор**. Если метод действия имеет параметр с именем Id, затем ASP.NET MVC автоматически передаст сегмент URL-адреса для вас как параметр.</span><span class="sxs-lookup"><span data-stu-id="13d1d-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="13d1d-319">В URL-АДРЕСЕ **Store/сведения/5**, **идентификатор** будут интерпретироваться как **5**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="13d1d-320">Изменение **сведения** метод **StoreController**, добавление **int** параметр с именем **идентификатор**. Чтобы сделать это, замените **сведения** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="13d1d-321">(Code Snippet - *основы ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="13d1d-322">Задача 4 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="13d1d-323">В этой задаче будет попробовать приложение в веб-браузер и использовать **идентификатор** параметра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="13d1d-324">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-325">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="13d1d-326">Изменить URL-адрес */Store/Details/5* чтобы убедиться, что действие получает параметр id.</span><span class="sxs-lookup"><span data-stu-id="13d1d-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="13d1d-327">![Просмотр StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "просмотра StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="13d1d-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="13d1d-328">*Просмотр /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="13d1d-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="13d1d-329">Упражнение 4: Создание представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="13d1d-330">Пока возврат строк из действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="13d1d-331">Несмотря на то, что это удобный способ Общие сведения о работе контроллеров, это не как создаются реальных веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="13d1d-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="13d1d-332">Представления — это компоненты, которые больше подходят для создания HTML в браузер с помощью файлов шаблонов.</span><span class="sxs-lookup"><span data-stu-id="13d1d-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="13d1d-333">В этом упражнении вы узнаете, как добавить главную страницу макета для настройки шаблона для типичного содержимого HTML, таблицу стилей, чтобы улучшить внешний вид сайта "и" шаблон представления, чтобы включить HomeController для возврата HTML.</span><span class="sxs-lookup"><span data-stu-id="13d1d-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="13d1d-334">Задача 1 - изменение файла \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="13d1d-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="13d1d-335">Файл **~/Views/Shared/\_layout.cshtml** позволяет настроить шаблон для общего HTML для всего веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="13d1d-336">В этой задаче вы добавите Главная страница макета с заголовком распространенных со ссылками в область домашней страницы и Store.</span><span class="sxs-lookup"><span data-stu-id="13d1d-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="13d1d-337">Если это еще не открыто, запустите **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="13d1d-338">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="13d1d-339">В диалоговом окне Открытие проекта, перейдите к **Source\Ex04 CreatingAView\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="13d1d-340">Кроме того вы можете продолжить с решением, полученный после выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="13d1d-341">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="13d1d-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13d1d-342">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13d1d-343">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="13d1d-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13d1d-344">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13d1d-345">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13d1d-346">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13d1d-347">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="13d1d-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="13d1d-348">Файл  <strong>\_layout.cshtml</strong> содержит макет контейнера HTML для всех страниц на сайте.</span><span class="sxs-lookup"><span data-stu-id="13d1d-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="13d1d-349">Он включает в себя <strong>&lt;html&gt;</strong> элемент для HTML-ответа, а также <strong>&lt;head&gt;</strong> и <strong>&lt;текст&gt;</strong> элементы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="13d1d-350"><strong>@RenderBody()</strong> в HTML-код тела идентификации регионов этого представления, шаблоны будут иметь возможность заполнить динамического содержимого.</span><span class="sxs-lookup"><span data-stu-id="13d1d-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="13d1d-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="13d1d-352">Добавьте общие заголовок со ссылками в область домашней страницы и Store на всех страницах сайта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="13d1d-353">Для этого добавьте следующий код ниже &lt;текст&gt; инструкции.</span><span class="sxs-lookup"><span data-stu-id="13d1d-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="13d1d-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="13d1d-355">Включить div для его отображения текста каждой страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="13d1d-356">Замените  <strong>@RenderBody()</strong> следующим кодом higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="13d1d-357">Знаете ли вы?</span><span class="sxs-lookup"><span data-stu-id="13d1d-357">Did you know?</span></span> <span data-ttu-id="13d1d-358">Visual Studio 2012 появилось фрагменты, которые позволяют легко добавлять часто используемый код в HTML, файлы кода и многое другое!</span><span class="sxs-lookup"><span data-stu-id="13d1d-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="13d1d-359">Проверим, введя **&lt;div&gt;** и нажав клавишу **ВКЛАДКЕ** дважды, чтобы вставить полный **div** тега.</span><span class="sxs-lookup"><span data-stu-id="13d1d-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="13d1d-360">Задача 2 - Добавление таблицы стилей CSS</span><span class="sxs-lookup"><span data-stu-id="13d1d-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="13d1d-361">Шаблон пустого проекта включает в себя очень упрощенный CSS-файл, который просто содержатся стили, используемые для отображения основных форм и проверки сообщений.</span><span class="sxs-lookup"><span data-stu-id="13d1d-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="13d1d-362">Вы будете использовать дополнительные CSS и изображений (потенциально в конструкторе), чтобы улучшить внешний вид сайта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="13d1d-363">В этой задаче вы добавите таблицу стилей CSS для определения стилей узла.</span><span class="sxs-lookup"><span data-stu-id="13d1d-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="13d1d-364">CSS-файл и образов, включенных в **Source\Assets\Content** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="13d1d-365">Чтобы добавить их в приложение, перетащите их содержимое из **Windows Explorer** в окно **обозревателе решений** в Visual Studio Express для веб-приложений, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="13d1d-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="13d1d-366">![Перетаскивание содержимое стиля](aspnet-mvc-4-fundamentals/_static/image12.png "перетаскивания содержимое стиля")</span><span class="sxs-lookup"><span data-stu-id="13d1d-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="13d1d-367">*Перетаскивание содержимое стиля*</span><span class="sxs-lookup"><span data-stu-id="13d1d-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="13d1d-368">Предупреждение появится диалоговое окно, требующее подтверждения для замены **Site.css** файлов и некоторых существующих образов.</span><span class="sxs-lookup"><span data-stu-id="13d1d-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="13d1d-369">Проверьте **применить ко всем элементам** и нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="13d1d-370">Задача 3 - Добавление шаблона представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="13d1d-371">В этой задаче вы добавите шаблона представления для создания HTML-ответа, который будет использоваться макет главной страницы и добавить CSS в этом упражнении.</span><span class="sxs-lookup"><span data-stu-id="13d1d-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="13d1d-372">Для использования шаблона представления, при переходе на домашнюю страницу, сначала необходимо будет указать, что вместо возврата строки, **HomeController Index** метод возвратит **представление**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="13d1d-373">Откройте **HomeController** класса и измените его **индекс** метод для возврата **ActionResult**, и вернуть его **View()**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="13d1d-374">(Code Snippet - *основы ASP.NET MVC 4 - Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="13d1d-375">Теперь необходимо добавить соответствующий шаблон представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="13d1d-376">Чтобы сделать это, **щелкните правой кнопкой мыши** внутри **индекс** метода действия и выберите **Добавление представления**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="13d1d-377">Это приведет к появлению **Добавление представления** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="13d1d-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="13d1d-378">![Добавление представления из в метод Index](aspnet-mvc-4-fundamentals/_static/image13.png "Добавление представления из в метод Index")</span><span class="sxs-lookup"><span data-stu-id="13d1d-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="13d1d-379">*Добавление представления из в метод Index*</span><span class="sxs-lookup"><span data-stu-id="13d1d-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="13d1d-380">**Добавление представления** появится диалоговое окно, чтобы создать файл шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="13d1d-381">По умолчанию это диалоговое окно предварительно заполняет имя шаблона представления, чтобы он соответствовал метод действия, который будет его использовать.</span><span class="sxs-lookup"><span data-stu-id="13d1d-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="13d1d-382">Так как вы использовали **Добавление представления** контекстного меню в **индекс** метода действия в HomeController, **Добавление представления** диалоговое окно имеет индекс в качестве имени представления по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="13d1d-383">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-383">Click **Add**.</span></span>

    <span data-ttu-id="13d1d-384">![Диалоговое окно добавления представления](aspnet-mvc-4-fundamentals/_static/image14.png "диалоговое окно добавления представления")</span><span class="sxs-lookup"><span data-stu-id="13d1d-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="13d1d-385">*Диалоговое окно добавления представления*</span><span class="sxs-lookup"><span data-stu-id="13d1d-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="13d1d-386">Visual Studio создает **Index.cshtml** шаблон представления внутри **Views\Home** папку, а затем открывает его.</span><span class="sxs-lookup"><span data-stu-id="13d1d-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="13d1d-387">![Индекс представление, созданное главной](aspnet-mvc-4-fundamentals/_static/image15.png "Главная индекс представление, созданное")</span><span class="sxs-lookup"><span data-stu-id="13d1d-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="13d1d-388">*Домашнее представление индекс создан*</span><span class="sxs-lookup"><span data-stu-id="13d1d-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-389">имя и расположение **Index.cshtml** файл относится и соответствует об именовании ASP.NET MVC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="13d1d-390">Папка \Views\**Главная** соответствует имени контроллера (**Главная** контроллера).</span><span class="sxs-lookup"><span data-stu-id="13d1d-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="13d1d-391">Имя шаблона представления (**индекс**), соответствующий метод действия контроллера, который будет применяться для отображения представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="13d1d-392">Таким образом, ASP.NET MVC позволяет избежать необходимости явно указывать имя или расположение шаблона представления при использовании это соглашение об именовании, чтобы вернуть представление.</span><span class="sxs-lookup"><span data-stu-id="13d1d-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="13d1d-393">Созданный шаблон представления основана на  **\_layout.cshtml** шаблон, определенный ранее.</span><span class="sxs-lookup"><span data-stu-id="13d1d-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="13d1d-394">Обновить свойство ViewBag.Title для **Главная**и изменить основное содержимое для **это домашняя страница**, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="13d1d-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="13d1d-395">Выберите **MvcMusicStore** проекта в обозревателе решений и нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="13d1d-396">Задача 4: проверка</span><span class="sxs-lookup"><span data-stu-id="13d1d-396">Task 4: Verification</span></span>

<span data-ttu-id="13d1d-397">Чтобы проверить, что вы правильно выполнили все действия, описанные в предыдущем упражнении, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="13d1d-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="13d1d-398">Приложение, открытое в браузере следует отметить, что:</span><span class="sxs-lookup"><span data-stu-id="13d1d-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="13d1d-399">Метод действия Index HomeController обнаружено и отображено **\Views\Home\Index.cshtml** просмотреть шаблон, в том случае, несмотря на то, что код вызывается **return View()**, так как шаблон представления стандартного соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="13d1d-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="13d1d-400">Домашняя страница отображает приветственное сообщение, определенные в **\Views\Home\Index.cshtml** шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="13d1d-401">На домашней странице используется  **\_layout.cshtml** шаблона, и поэтому содержится приветственное сообщение в формате HTML стандартный узла.</span><span class="sxs-lookup"><span data-stu-id="13d1d-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="13d1d-402">![Домашней представления индекса, используя определенные LayoutPage и стиль](aspnet-mvc-4-fundamentals/_static/image16.png "Главная представления индекса, используя определенные LayoutPage и стиль")</span><span class="sxs-lookup"><span data-stu-id="13d1d-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="13d1d-403">*Домашнее представление индекса, используя определенные LayoutPage и стиль*</span><span class="sxs-lookup"><span data-stu-id="13d1d-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="13d1d-404">Упражнение 5: Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="13d1d-405">На данный момент внесенные представлений отображения жестко HTML, но для создания динамических веб-приложений, этот шаблон должен получать сведения из контроллера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="13d1d-406">Один общий метод, позволяющий использовать для этой цели является **ViewModel** шаблон, который позволяет контроллеру упаковать все сведения, необходимые для создания соответствующих HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="13d1d-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="13d1d-407">В этом упражнении вы узнаете, как создать класс модели представления и добавить необходимые свойства: число жанров в хранилище и список этих жанров.</span><span class="sxs-lookup"><span data-stu-id="13d1d-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="13d1d-408">Требуется обновить StoreController использовать созданный ViewModel, и наконец, вы создадите новый шаблон представления, упомянутые свойства отобразятся на странице.</span><span class="sxs-lookup"><span data-stu-id="13d1d-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="13d1d-409">Задача 1 - Создание класса модели представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="13d1d-410">В этой задаче вы создадите класс модели представления, которые реализуют сценарий список жанров Store.</span><span class="sxs-lookup"><span data-stu-id="13d1d-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="13d1d-411">Если это еще не открыто, запустите **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="13d1d-412">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="13d1d-413">В диалоговом окне Открытие проекта, перейдите к **Source\Ex05 CreatingAViewModel\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="13d1d-414">Кроме того вы можете продолжить с решением, полученный после выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="13d1d-415">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="13d1d-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13d1d-416">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13d1d-417">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="13d1d-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13d1d-418">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13d1d-419">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13d1d-420">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13d1d-421">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="13d1d-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="13d1d-422">Создание **ViewModels** папку для хранения ViewModel.</span><span class="sxs-lookup"><span data-stu-id="13d1d-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="13d1d-423">Чтобы сделать это, щелкните правой кнопкой мыши верхнего уровня **MvcMusicStore** проекта, выберите **добавить** и затем **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="13d1d-424">![Добавления новой папки](aspnet-mvc-4-fundamentals/_static/image17.png "добавления новой папки")</span><span class="sxs-lookup"><span data-stu-id="13d1d-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="13d1d-425">*Добавление новой папки*</span><span class="sxs-lookup"><span data-stu-id="13d1d-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="13d1d-426">Назовите папку *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="13d1d-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="13d1d-427">![ViewModels папку в обозревателе решений](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels папку в обозревателе решений")</span><span class="sxs-lookup"><span data-stu-id="13d1d-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="13d1d-428">*ViewModels папку в обозревателе решений*</span><span class="sxs-lookup"><span data-stu-id="13d1d-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="13d1d-429">Создание **ViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="13d1d-430">Чтобы сделать это, щелкните правой кнопкой мыши **ViewModels** недавно созданные папки выберите **добавить** и затем **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="13d1d-431">В разделе **кода**, выберите **класс** элемента и назовите файл *StoreIndexViewModel.cs*, затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="13d1d-432">![Добавление нового класса](aspnet-mvc-4-fundamentals/_static/image19.png "добавив новый класс")</span><span class="sxs-lookup"><span data-stu-id="13d1d-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="13d1d-433">*Добавление нового класса*</span><span class="sxs-lookup"><span data-stu-id="13d1d-433">*Adding a new Class*</span></span>

    <span data-ttu-id="13d1d-434">![Создание класса StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel создание класса")</span><span class="sxs-lookup"><span data-stu-id="13d1d-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="13d1d-435">*Создание класса StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="13d1d-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="13d1d-436">Задача 2 - Добавление свойств в классе модели представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="13d1d-437">Есть два параметра, передаваемые из StoreController этот шаблон для формирования ожидаемого времени ответа HTML: число жанров в хранилище и список этих жанров.</span><span class="sxs-lookup"><span data-stu-id="13d1d-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="13d1d-438">В этой задаче вы добавите эти 2 свойства **StoreIndexViewModel** класса: **NumberOfGenres** (целое число) и **жанров** (список строк).</span><span class="sxs-lookup"><span data-stu-id="13d1d-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="13d1d-439">Добавить **NumberOfGenres** и **жанров** свойства **StoreIndexViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="13d1d-440">Чтобы сделать это, добавьте следующие строки 2 в определение класса:</span><span class="sxs-lookup"><span data-stu-id="13d1d-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="13d1d-441">(Code Snippet - *основы 4 MVC для ASP.NET - свойства Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="13d1d-442">**{Get; set;}**  нотации использует C# автоматически реализуемые свойства компонента.</span><span class="sxs-lookup"><span data-stu-id="13d1d-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="13d1d-443">Он предоставляет преимущества свойства без необходимости нам для объявления резервным полем.</span><span class="sxs-lookup"><span data-stu-id="13d1d-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="13d1d-444">Задача 3 - обновление StoreController использовать StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="13d1d-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="13d1d-445">**StoreIndexViewModel** класс инкапсулирует сведения, необходимые для передачи из **StoreController** **индекс** метод для шаблона представления для создания ответа .</span><span class="sxs-lookup"><span data-stu-id="13d1d-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="13d1d-446">В этой задаче вы обновите **StoreController** использовать **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="13d1d-447">Откройте **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="13d1d-448">![Открыв класс StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController открывающей класса")</span><span class="sxs-lookup"><span data-stu-id="13d1d-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="13d1d-449">*Класс StoreController открытия*</span><span class="sxs-lookup"><span data-stu-id="13d1d-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="13d1d-450">Чтобы использовать **StoreIndexViewModel** класса из **StoreController**, добавьте следующее пространство имен в верхней части **StoreController** кода:</span><span class="sxs-lookup"><span data-stu-id="13d1d-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="13d1d-451">(Code Snippet - *основы 4 MVC для ASP.NET — использование ViewModel StoreIndexViewModel Ex5*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="13d1d-452">Изменение **StoreController** **индекс** метода действия, так что он создает и заполняет **StoreIndexViewModel** и затем передает его шаблону представления для Создание HTML-ответа с ним.</span><span class="sxs-lookup"><span data-stu-id="13d1d-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-453">В лаборатории &quot;модели MVC ASP.NET и доступа к данным&quot; вы напишете код, извлекающий список жанров магазина из базы данных.</span><span class="sxs-lookup"><span data-stu-id="13d1d-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="13d1d-454">В следующем коде создается **списка** жанров фиктивных данных, который будет заполнять **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="13d1d-455">После создания и настройки **StoreIndexViewModel** объекта, он будет передан в качестве аргумента для **представление** метод.</span><span class="sxs-lookup"><span data-stu-id="13d1d-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="13d1d-456">Это означает, что этот шаблон будет использоваться этот объект для создания HTML-ответа с ним.</span><span class="sxs-lookup"><span data-stu-id="13d1d-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="13d1d-457">Замените **индекс** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="13d1d-458">(Code Snippet - *основы 4 MVC для ASP.NET - метод Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="13d1d-459">Если вы не знакомы с C#, может предположить, что **var** означает, что **viewModel** переменной с поздним связыванием.</span><span class="sxs-lookup"><span data-stu-id="13d1d-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="13d1d-460">Неправильный - компилятор C# использует определение типа зависимости от того, что можно присвоить переменной, чтобы определять **viewModel** имеет тип **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="13d1d-461">Кроме того, скомпилировав локальной **viewModel** переменной как **StoreIndexViewModel** типа get проверки во время компиляции и поддержка в редакторе кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="13d1d-462">Задача 4 - Создание шаблона представления, который использует StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="13d1d-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="13d1d-463">В этой задаче вы создадите представление шаблон, который будет использовать StoreIndexViewModel объекта, переданного из контроллера для отображения списка жанров.</span><span class="sxs-lookup"><span data-stu-id="13d1d-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="13d1d-464">Прежде чем создавать новый шаблон представления, давайте создадим проект, чтобы **Добавление диалогового окна представления** знает о **StoreIndexViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="13d1d-465">Постройте проект, выбрав **построения** пункта меню и затем **построения MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="13d1d-466">![Построение проекта](aspnet-mvc-4-fundamentals/_static/image22.png "построение проекта")</span><span class="sxs-lookup"><span data-stu-id="13d1d-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="13d1d-467">*Построение проекта*</span><span class="sxs-lookup"><span data-stu-id="13d1d-467">*Building the project*</span></span>
2. <span data-ttu-id="13d1d-468">Создание шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-468">Create a new View template.</span></span> <span data-ttu-id="13d1d-469">Чтобы сделать это, щелкните правой кнопкой мыши внутри **индекс** метод и выберите **Добавление представления**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="13d1d-470">![Добавление представления](aspnet-mvc-4-fundamentals/_static/image23.png "Добавление представления")</span><span class="sxs-lookup"><span data-stu-id="13d1d-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="13d1d-471">*Добавление представления*</span><span class="sxs-lookup"><span data-stu-id="13d1d-471">*Adding a View*</span></span>
3. <span data-ttu-id="13d1d-472">Так как **Добавление диалогового окна представления** был вызван из **StoreController**, добавит этот шаблон по умолчанию в **\Views\Store\Index.cshtml** файла.</span><span class="sxs-lookup"><span data-stu-id="13d1d-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="13d1d-473">Проверьте **создать строго типизированных представление** флажок, а затем выберите **StoreIndexViewModel** как **класс модели**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="13d1d-474">Кроме того, убедитесь, что выбран обработчик представлений **Razor**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="13d1d-475">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-475">Click **Add**.</span></span>

    <span data-ttu-id="13d1d-476">![Диалоговое окно добавления представления](aspnet-mvc-4-fundamentals/_static/image24.png "диалоговое окно добавления представления")</span><span class="sxs-lookup"><span data-stu-id="13d1d-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="13d1d-477">*Диалоговое окно добавления представления*</span><span class="sxs-lookup"><span data-stu-id="13d1d-477">*Add View Dialog*</span></span>

    <span data-ttu-id="13d1d-478">**\Views\Store\Index.cshtml** файл шаблона представления создается и открывается.</span><span class="sxs-lookup"><span data-stu-id="13d1d-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="13d1d-479">В зависимости от сведений, предоставленных **Добавление представления** диалоговое окно на предыдущем шаге, представления, шаблон будет ожидать **StoreIndexViewModel** экземпляру, что данные, используемые для создания HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="13d1d-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="13d1d-480">Обратите внимание, что шаблон наследует `ViewPage<musicstore.viewmodels.storeindexviewmodel>` в C#.</span><span class="sxs-lookup"><span data-stu-id="13d1d-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="13d1d-481">Задача 5 - Обновление представления шаблона</span><span class="sxs-lookup"><span data-stu-id="13d1d-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="13d1d-482">В этой задаче будет обновить представление шаблон, созданный в последней задаче для получения числа жанров и их имен в пределах страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="13d1d-483">Вы воспользуетесь @ синтаксис (часто обозначается как &quot;фрагменты кода&quot;) для выполнения кода в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="13d1d-484">В **Index.cshtml** файл, в **Store** папка, замените его код на следующий:</span><span class="sxs-lookup"><span data-stu-id="13d1d-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="13d1d-485">Цикл по списку жанр в **StoreIndexViewModel** и создать HTML **&lt;ul&gt;** список с помощью **foreach** цикла.</span><span class="sxs-lookup"><span data-stu-id="13d1d-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="13d1d-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="13d1d-487">Нажмите клавишу **F5** для запуска приложения и Обзор **/Store**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="13d1d-488">Вы увидите список жанров, переданный **StoreIndexViewModel** объекта из **StoreController** в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="13d1d-489">![Представление со списком жанров](aspnet-mvc-4-fundamentals/_static/image26.png "представления, отображающее список жанров")</span><span class="sxs-lookup"><span data-stu-id="13d1d-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="13d1d-490">*Представление со списком жанров*</span><span class="sxs-lookup"><span data-stu-id="13d1d-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="13d1d-491">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="13d1d-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="13d1d-492">Упражнение 6: Использование параметров в представлении</span><span class="sxs-lookup"><span data-stu-id="13d1d-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="13d1d-493">В упражнении 3 вы узнали, как передавать параметры к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="13d1d-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="13d1d-494">В этом упражнении вы узнаете, как использовать эти параметры в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="13d1d-495">Для этой цели вы познакомитесь сначала классов модели, которые помогут вам управлять логику данных и домена.</span><span class="sxs-lookup"><span data-stu-id="13d1d-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="13d1d-496">Кроме того вы узнаете, как создать ссылки на страницы в приложении ASP.NET MVC, не беспокоясь вещей, такие как кодирование URL-путей.</span><span class="sxs-lookup"><span data-stu-id="13d1d-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="13d1d-497">Задача 1 - Добавление классов модели</span><span class="sxs-lookup"><span data-stu-id="13d1d-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="13d1d-498">В отличие от модели ViewModel, в которой создаются только для передачи данных из контроллера в представление классов модели создаются для хранения и управления логику данных и домена.</span><span class="sxs-lookup"><span data-stu-id="13d1d-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="13d1d-499">В этой задаче вы добавите двух классов модели для представления этих концепций: **жанр** и **альбома**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="13d1d-500">Если это еще не открыто, запустите **VS Express для Web**</span><span class="sxs-lookup"><span data-stu-id="13d1d-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="13d1d-501">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="13d1d-502">В диалоговом окне Открытие проекта, перейдите к **Source\Ex06 UsingParametersInView\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="13d1d-503">Кроме того вы можете продолжить с решением, полученный после выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="13d1d-504">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="13d1d-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13d1d-505">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13d1d-506">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="13d1d-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13d1d-507">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13d1d-508">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13d1d-509">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13d1d-510">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="13d1d-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="13d1d-511">Добавить **жанр** класс модели.</span><span class="sxs-lookup"><span data-stu-id="13d1d-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="13d1d-512">Чтобы сделать это, щелкните правой кнопкой мыши **моделей** папку в **обозревателе решений**выберите **добавить** и затем **новый элемент** параметр.</span><span class="sxs-lookup"><span data-stu-id="13d1d-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="13d1d-513">В разделе **кода**, выберите **класс** элемента и назовите файл *Genre.cs*, затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="13d1d-514">![Добавление класса](aspnet-mvc-4-fundamentals/_static/image27.png "Добавление класса")</span><span class="sxs-lookup"><span data-stu-id="13d1d-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="13d1d-515">*Добавление нового элемента*</span><span class="sxs-lookup"><span data-stu-id="13d1d-515">*Adding a new item*</span></span>

    <span data-ttu-id="13d1d-516">![Добавление класса модели жанр](aspnet-mvc-4-fundamentals/_static/image28.png "Добавление класса модели жанра")</span><span class="sxs-lookup"><span data-stu-id="13d1d-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="13d1d-517">*Добавление класса модели жанра*</span><span class="sxs-lookup"><span data-stu-id="13d1d-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="13d1d-518">Добавить **имя** свойство к классу жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="13d1d-519">Чтобы сделать это, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="13d1d-519">To do this, add the following code:</span></span>

    <span data-ttu-id="13d1d-520">(Code Snippet - *основы ASP.NET MVC 4 - жанр Ex6*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="13d1d-521">Таким же способом, что и раньше, добавьте **альбома** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="13d1d-522">Чтобы сделать это, щелкните правой кнопкой мыши **моделей** папку в **обозревателе решений**выберите **добавить** и затем **новый элемент** параметр.</span><span class="sxs-lookup"><span data-stu-id="13d1d-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="13d1d-523">В разделе **кода**, выберите **класс** элемента и назовите файл *Album.cs*, затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="13d1d-524">Добавьте в класс Album два свойства: **жанр** и **Title**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="13d1d-525">Чтобы сделать это, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="13d1d-525">To do this, add the following code:</span></span>

    <span data-ttu-id="13d1d-526">(Code Snippet - *основы ASP.NET MVC 4 - альбома Ex6*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="13d1d-527">Задача 2 - Добавление StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="13d1d-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="13d1d-528">Объект **StoreBrowseViewModel** будет использоваться в этой задаче для отображения и альбомы, которые соответствуют выбранного жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="13d1d-529">В этой задаче вы создадите этот класс и затем добавить два свойства для обработки **жанр** и его **альбома**в списке.</span><span class="sxs-lookup"><span data-stu-id="13d1d-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="13d1d-530">Добавить **StoreBrowseViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="13d1d-531">Чтобы сделать это, щелкните правой кнопкой мыши **ViewModels** папку в **обозревателе решений**выберите **добавить** и затем **новый элемент** параметр.</span><span class="sxs-lookup"><span data-stu-id="13d1d-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="13d1d-532">В разделе **кода**, выберите **класс** элемента и назовите файл *StoreBrowseViewModel.cs*, затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="13d1d-533">Добавьте ссылку в моделях в **StoreBrowseViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="13d1d-534">Чтобы сделать это, добавьте следующее пространство имен:</span><span class="sxs-lookup"><span data-stu-id="13d1d-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="13d1d-535">(Code Snippet - *основы ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="13d1d-536">Добавьте два свойства **StoreBrowseViewModel** класса: **жанр** и **альбомов**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="13d1d-537">Чтобы сделать это, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="13d1d-537">To do this, add the following code:</span></span>

    <span data-ttu-id="13d1d-538">(Code Snippet - *основы ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="13d1d-539">Что такое **списка&lt;альбома&gt;**  ?: использует это определение **списка&lt;T&gt;**  типа, где **T** ограничивает тип для этих элементов этого **списка** принадлежат, в этом случае **альбома** (или один из его потомков).</span><span class="sxs-lookup"><span data-stu-id="13d1d-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="13d1d-540">Эта возможность проектировать классы и методы, спецификация которых отложена из одного или нескольких типов пока класс или метод объявляется и создается в клиентском коде входит в состав языка C# называется **универсальные шаблоны**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="13d1d-541">**Список&lt;T&gt;**  является универсальным, равное **ArrayList** введите и доступен в **System.Collections.Generic** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="13d1d-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="13d1d-542">Одним из преимуществ использования **универсальные шаблоны** , так как указан тип, вы не нужны для обеспечения проверки проверки операций, таких как элементы в приведении типа **альбома** как это делается с помощью **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="13d1d-543">Задача 3 - использование нового ViewModel в StoreController</span><span class="sxs-lookup"><span data-stu-id="13d1d-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="13d1d-544">В этой задаче будет изменено **StoreController** **Обзор** и **сведения** методы действий для использования нового **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="13d1d-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="13d1d-545">Добавьте ссылку на папку Models в **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="13d1d-546">Для этого разверните **контроллеров** папку в **обозревателе решений** и откройте **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="13d1d-547">Затем добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="13d1d-547">Then add the following code:</span></span>

    <span data-ttu-id="13d1d-548">(Code Snippet - *основы ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="13d1d-549">Замените **Обзор** метода действия для использования **StoreViewBrowseController** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="13d1d-550">Вы создадите жанр фильма и два новых объектов альбомов с фиктивными данными (в следующий практический семинар будет использовать реальные данные из базы данных).</span><span class="sxs-lookup"><span data-stu-id="13d1d-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="13d1d-551">Чтобы сделать это, замените **Обзор** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="13d1d-552">(Code Snippet - *основы ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="13d1d-553">Замените **сведения** метода действия для использования **StoreViewBrowseController** класса.</span><span class="sxs-lookup"><span data-stu-id="13d1d-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="13d1d-554">Вы создадите новый **альбома** возвращался к **представление**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="13d1d-555">Чтобы сделать это, замените **сведения** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="13d1d-556">(Code Snippet - *основы ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="13d1d-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="13d1d-557">Задача 4 - Добавление шаблона представления обзора</span><span class="sxs-lookup"><span data-stu-id="13d1d-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="13d1d-558">В этой задаче вы добавите **Обзор** представление для отображения альбомов, найден для определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="13d1d-559">Прежде чем создавать новый шаблон представления, следует построить проект, чтобы **Добавление представления** диалоговое окно знает о **ViewModel** класс для использования.</span><span class="sxs-lookup"><span data-stu-id="13d1d-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="13d1d-560">Постройте проект, выбрав **построения** пункта меню и затем **построения MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="13d1d-561">Добавить **Обзор** представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-561">Add a **Browse** View.</span></span> <span data-ttu-id="13d1d-562">Чтобы сделать это, щелкните правой кнопкой мыши в **Обзор** метод действия **StoreController** и нажмите кнопку **Добавление представления**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="13d1d-563">В **Добавление представления** диалоговом окне убедитесь, что имя представления **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="13d1d-564">Проверьте **создать строго типизированное представление** флажок и выберите **StoreBrowseViewModel** из **класс модели** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="13d1d-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="13d1d-565">Оставьте в остальных полях значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="13d1d-566">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-566">Then click **Add**.</span></span>

    <span data-ttu-id="13d1d-567">![Добавление представления обзора](aspnet-mvc-4-fundamentals/_static/image29.png "Добавление представления обзора")</span><span class="sxs-lookup"><span data-stu-id="13d1d-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="13d1d-568">*Добавление представления обзора*</span><span class="sxs-lookup"><span data-stu-id="13d1d-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="13d1d-569">Изменить **Browse.cshtml** для отображения сведений жанра, доступ к **StoreBrowseViewModel** объект, передаваемый в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="13d1d-570">Для этого замените содержимое следующим: (C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="13d1d-571">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="13d1d-572">В этой задаче вы проверите, **Обзор** метод извлекает альбомов из **Обзор** действие метода.</span><span class="sxs-lookup"><span data-stu-id="13d1d-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="13d1d-573">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-574">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-574">The project starts in the Home page.</span></span> <span data-ttu-id="13d1d-575">Изменить URL-адрес   **/Store/Обзор? Жанр = Disco** чтобы убедиться, что это действие возвращает два альбомов.</span><span class="sxs-lookup"><span data-stu-id="13d1d-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="13d1d-576">![Обзор Store Disco альбомов](aspnet-mvc-4-fundamentals/_static/image30.png "просмотра Disco альбомов Store")</span><span class="sxs-lookup"><span data-stu-id="13d1d-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="13d1d-577">*Просмотр Disco альбомов Store*</span><span class="sxs-lookup"><span data-stu-id="13d1d-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="13d1d-578">Задача 6 - Отображение сведений о определенный альбом</span><span class="sxs-lookup"><span data-stu-id="13d1d-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="13d1d-579">В этой задаче вы реализуете **Store и подробных** представление для отображения сведений о определенный альбом.</span><span class="sxs-lookup"><span data-stu-id="13d1d-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="13d1d-580">В этом Практическое лабораторное занятие, все, что будет отображаться альбома уже содержится в **представление** шаблона.</span><span class="sxs-lookup"><span data-stu-id="13d1d-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="13d1d-581">В этом случае вместо создания **StoreDetailsViewModel** класса, будет использовать текущий **StoreBrowseViewModel** шаблона, передавая ему альбома.</span><span class="sxs-lookup"><span data-stu-id="13d1d-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="13d1d-582">Закройте браузер, при необходимости, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="13d1d-583">Добавьте новый **сведения** просмотра для **StoreController** **сведения** метода действия.</span><span class="sxs-lookup"><span data-stu-id="13d1d-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="13d1d-584">Чтобы сделать это, щелкните правой кнопкой мыши **сведения** метод в **StoreController** класса и нажмите кнопку **Добавление представления**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="13d1d-585">В **Добавление представления** диалоговое окно, убедитесь, что **Имя_представления** — **сведения**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="13d1d-586">Проверьте **создать строго типизированное представление** флажок и выберите **альбома** из **класс модели** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="13d1d-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="13d1d-587">Оставьте в остальных полях значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="13d1d-588">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-588">Then click **Add**.</span></span> <span data-ttu-id="13d1d-589">Это будет создавать и открывать **\Views\Store\Details.cshtml** файла.</span><span class="sxs-lookup"><span data-stu-id="13d1d-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="13d1d-590">![Добавление представления сведений о](aspnet-mvc-4-fundamentals/_static/image31.png "Добавление представления сведений")</span><span class="sxs-lookup"><span data-stu-id="13d1d-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="13d1d-591">*Добавление представления сведений*</span><span class="sxs-lookup"><span data-stu-id="13d1d-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="13d1d-592">Изменить **Details.cshtml** файл для отображения сведений о диске, доступ к **альбома** объект, передаваемый в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="13d1d-593">Для этого замените содержимое следующим: (C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="13d1d-594">Задача 7 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="13d1d-595">В этой задаче вы проверите, **сведения** представление получает сведения о альбома из **подробно действие** метод.</span><span class="sxs-lookup"><span data-stu-id="13d1d-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="13d1d-596">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-597">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="13d1d-598">Изменить URL-адрес **/Store/Details/5** Чтобы проверить сведения об альбоме.</span><span class="sxs-lookup"><span data-stu-id="13d1d-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="13d1d-599">![Просмотр сведений альбомов](aspnet-mvc-4-fundamentals/_static/image32.png "просмотра альбомов детализации")</span><span class="sxs-lookup"><span data-stu-id="13d1d-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="13d1d-600">*Просмотр сведений альбома*</span><span class="sxs-lookup"><span data-stu-id="13d1d-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="13d1d-601">Задача 8. Добавление ссылки между страницами</span><span class="sxs-lookup"><span data-stu-id="13d1d-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="13d1d-602">В этой задаче вы добавите ссылку в представлении Store должна иметься ссылка в имени каждого жанра к соответствующему **/Store/обзора** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="13d1d-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="13d1d-603">Таким образом, если щелкнуть жанр фильма, например **Disco**, он откроется с **/Store/обзора? жанр = Disco** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="13d1d-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="13d1d-604">Закройте браузер, при необходимости, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="13d1d-605">Обновление **индекс** страницу, чтобы добавить ссылку на **Обзор** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="13d1d-606">Для этого в **обозревателе решений** разверните **представления** папку, а затем **Store** папку и дважды щелкните **Index.cshtml** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="13d1d-607">Добавьте ссылку на представление обзора, указывающее, выбранного жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="13d1d-608">Чтобы сделать это, замените следующий выделенный код в **&lt;li&gt;** теги: (C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="13d1d-609">другой подход будет связывание непосредственно на страницу, с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="13d1d-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="13d1d-610">&lt;a href =&quot;/Store/обзора? жанр =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="13d1d-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="13d1d-611">Несмотря на то, что этот подход работает, он зависит от строки жестко.</span><span class="sxs-lookup"><span data-stu-id="13d1d-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="13d1d-612">При переименовании позже контроллера, необходимо вручную изменить эту инструкцию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="13d1d-613">Лучшим вариантом будет его использовать **вспомогательный метод HTML** метод.</span><span class="sxs-lookup"><span data-stu-id="13d1d-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="13d1d-614">ASP.NET MVC включает в себя метод вспомогательный метод HTML, который доступен для выполнения таких задач.</span><span class="sxs-lookup"><span data-stu-id="13d1d-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="13d1d-615">**Html.ActionLink()** вспомогательный метод позволяет легко создавать HTML **&lt;&gt;** ссылки, убедиться, пути URL-адресов осуществляется должным образом в кодировке URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="13d1d-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="13d1d-616">Htlm.ActionLink имеет несколько перегрузок.</span><span class="sxs-lookup"><span data-stu-id="13d1d-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="13d1d-617">В этом упражнении вам понадобится использовать одно, которое принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="13d1d-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="13d1d-618">Текст ссылки, который будет отображаться название жанра</span><span class="sxs-lookup"><span data-stu-id="13d1d-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="13d1d-619">Имя действия контроллера (**Обзор**)</span><span class="sxs-lookup"><span data-stu-id="13d1d-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="13d1d-620">Значения параметра, указав оба имя маршрута (**жанр**) и значение (**название жанра**)</span><span class="sxs-lookup"><span data-stu-id="13d1d-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="13d1d-621">Задача 9 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="13d1d-622">В этой задаче вы проверите, что каждого жанра отображается со ссылкой на соответствующий **/Store/обзора** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="13d1d-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="13d1d-623">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-624">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-624">The project starts in the Home page.</span></span> <span data-ttu-id="13d1d-625">Изменить URL-адрес **/Store** чтобы убедиться, что каждого жанра ссылки на соответствующие **/Store/обзора** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="13d1d-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="13d1d-626">![Просмотр жанров со ссылками на странице "Обзор"](aspnet-mvc-4-fundamentals/_static/image33.png "просмотра жанров со ссылками на странице \"Обзор\"")</span><span class="sxs-lookup"><span data-stu-id="13d1d-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="13d1d-627">*Просмотр жанров со ссылками на странице "Обзор"*</span><span class="sxs-lookup"><span data-stu-id="13d1d-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="13d1d-628">Задача 10 — с помощью коллекции динамические ViewModel для передачи значений</span><span class="sxs-lookup"><span data-stu-id="13d1d-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="13d1d-629">В этой задаче вы узнаете, как простой и эффективный способ передачи значений между контроллером и представлением, не внося никаких изменений в модели.</span><span class="sxs-lookup"><span data-stu-id="13d1d-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="13d1d-630">ASP.NET MVC 4 предоставляет коллекцию &quot;ViewModel&quot;, который можно назначить любое динамическое значение и получить доступ в контроллеры и представления также.</span><span class="sxs-lookup"><span data-stu-id="13d1d-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="13d1d-631">Теперь вы будет использовать динамическую коллекцию ViewBag для передачи списка &quot; **звездочками жанров** &quot; из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="13d1d-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="13d1d-632">Представление Store Index будет доступ к **ViewModel** и отображения информации.</span><span class="sxs-lookup"><span data-stu-id="13d1d-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="13d1d-633">Закройте браузер, при необходимости, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="13d1d-634">Откройте **StoreController.cs** и изменение **индекс** метод, чтобы создать список жанров звездочками в коллекцию ViewModel:</span><span class="sxs-lookup"><span data-stu-id="13d1d-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="13d1d-635">Можно также использовать синтаксис **ViewBag [&quot;Starred&quot;]** доступа к свойствам.</span><span class="sxs-lookup"><span data-stu-id="13d1d-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="13d1d-636">Значок звездочки **&quot;starred.png&quot;** включается в **Source\Assets\Images** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="13d1d-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="13d1d-637">Чтобы добавить его в приложение, перетащите их содержимое из **Windows Explorer** в окно **обозревателе решений** в Visual Web Developer Express, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="13d1d-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="13d1d-638">![Изображение типа "звезда" Добавление к решению](aspnet-mvc-4-fundamentals/_static/image34.png "изображение типа \"звезда\" Добавление в решение")</span><span class="sxs-lookup"><span data-stu-id="13d1d-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="13d1d-639">*В решение при добавлении изображения типа "звезда"*</span><span class="sxs-lookup"><span data-stu-id="13d1d-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="13d1d-640">Откройте представление **Store/Index.cshtml** и изменить содержимое.</span><span class="sxs-lookup"><span data-stu-id="13d1d-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="13d1d-641">Будет считывать &quot;звездочками&quot; свойство в **ViewBag** коллекции и узнайте имя текущего жанра, в списке.</span><span class="sxs-lookup"><span data-stu-id="13d1d-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="13d1d-642">В этом случае будет показывать значок звездочки прямо по ссылке жанра.</span><span class="sxs-lookup"><span data-stu-id="13d1d-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="13d1d-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="13d1d-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="13d1d-644">Задача 11. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="13d1d-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="13d1d-645">В этой задаче вы проверите, что со звездочками жанры отображать значок звездочки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="13d1d-646">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13d1d-647">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="13d1d-648">Изменить URL-адрес **/Store** для убедитесь, что каждый избранные жанр уважение метки:</span><span class="sxs-lookup"><span data-stu-id="13d1d-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="13d1d-649">![Просмотр жанров с со звездочками элементами](aspnet-mvc-4-fundamentals/_static/image35.png "жанров обзора с элементами со звездочками")</span><span class="sxs-lookup"><span data-stu-id="13d1d-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="13d1d-650">*Browsing жанров со звездочками элементов*</span><span class="sxs-lookup"><span data-stu-id="13d1d-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="13d1d-651">Упражнение 7: Краткая информация о новый шаблон ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="13d1d-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="13d1d-652">В этом упражнении будет изучено усовершенствования в шаблоны проектов ASP.NET MVC 4, воспользовавшись наиболее соответствующие функции нового шаблона.</span><span class="sxs-lookup"><span data-stu-id="13d1d-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="13d1d-653">Задача 1: Изучение данный шаблон приложения ASP.NET MVC 4 Internet</span><span class="sxs-lookup"><span data-stu-id="13d1d-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="13d1d-654">Если это еще не открыто, запустите **VS Express для Web**</span><span class="sxs-lookup"><span data-stu-id="13d1d-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="13d1d-655">Выберите **файл | Новые | Проект** команды меню.</span><span class="sxs-lookup"><span data-stu-id="13d1d-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="13d1d-656">В **новый проект** диалоговом окне выберите **Visual C# | Web** шаблона на левой панели дерева, а затем выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="13d1d-657">**Имя** проекта *MusicStore* и обновить **имя решения** для *начать*, затем выберите расположение (или оставьте значение по умолчанию) и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="13d1d-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="13d1d-658">![Создание нового проекта ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Создание нового проекта ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="13d1d-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="13d1d-659">*Создание нового проекта ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="13d1d-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="13d1d-660">В **создания проекта ASP.NET MVC 4** диалоговом окне выберите **веб-приложение** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="13d1d-661">Обратите внимание, что можно выбрать как подсистема просмотра Razor или ASPX.</span><span class="sxs-lookup"><span data-stu-id="13d1d-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="13d1d-662">![Создание нового приложения ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "Создание нового приложения ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="13d1d-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="13d1d-663">*Создание нового приложения ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="13d1d-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-664">Синтаксис Razor представлена в ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="13d1d-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="13d1d-665">Наша цель — чтобы свести к минимуму число знаков и нажатий клавиш в файл, позволяя быстро и жидкостей, рабочие процессы кодирования.</span><span class="sxs-lookup"><span data-stu-id="13d1d-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="13d1d-666">Razor использует существующие C# и Visual Basic (или другая) языковые навыки и обеспечивает синтаксис разметки шаблона, который позволяет awesome рабочего процесса создания HTML.</span><span class="sxs-lookup"><span data-stu-id="13d1d-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="13d1d-667">Нажмите клавишу **F5** Чтобы выполнить решение и просмотреть обновленный шаблон.</span><span class="sxs-lookup"><span data-stu-id="13d1d-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="13d1d-668">Вы можете проверить следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="13d1d-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="13d1d-669">**Шаблоны в современном стиле**</span><span class="sxs-lookup"><span data-stu-id="13d1d-669">**Modern-style templates**</span></span>

        <span data-ttu-id="13d1d-670">Шаблоны были обновлены, предоставляя дополнительные стили современных.</span><span class="sxs-lookup"><span data-stu-id="13d1d-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="13d1d-671">![Шаблоны ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled шаблонов в ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="13d1d-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="13d1d-672">*Шаблоны ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="13d1d-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="13d1d-673">**Адаптивная отрисовка**</span><span class="sxs-lookup"><span data-stu-id="13d1d-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="13d1d-674">Извлеките изменения размера окна браузера и обратите внимание на то, как макет страницы приспосабливается к изменениям для нового размера окна.</span><span class="sxs-lookup"><span data-stu-id="13d1d-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="13d1d-675">Эти шаблоны использовать методика адаптивной отрисовки для неправильно отображается на настольных и мобильных платформах без какой-либо настройки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="13d1d-676">![Шаблон проекта ASP.NET MVC 4 в другом браузере размеры](aspnet-mvc-4-fundamentals/_static/image39.png "шаблон проекта ASP.NET MVC 4 в другом браузере размеры")</span><span class="sxs-lookup"><span data-stu-id="13d1d-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="13d1d-677">*Шаблон проекта ASP.NET MVC 4 в другом браузере размеры*</span><span class="sxs-lookup"><span data-stu-id="13d1d-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="13d1d-678">Закройте браузер, чтобы остановить отладчик и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="13d1d-679">Теперь имеется возможность просматривать решения и некоторые новые функции ASP.NET MVC 4 в шаблоне проекта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="13d1d-680">![ASP.NET MVC 4 internet--шаблон проекта приложения-](aspnet-mvc-4-fundamentals/_static/image40.png "шаблон проекта приложения ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="13d1d-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="13d1d-681">*Шаблон проекта приложения ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="13d1d-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="13d1d-682">**Разметку HTML5**</span><span class="sxs-lookup"><span data-stu-id="13d1d-682">**HTML5 markup**</span></span>

       <span data-ttu-id="13d1d-683">Обзор представлений шаблона, чтобы узнать, Новая разметка темы, например открыть **About.cshtml** просматривать в **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="13d1d-684">![Новый шаблон, с помощью разметки Razor и HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "новый шаблон, с помощью разметки Razor и HTML5")</span><span class="sxs-lookup"><span data-stu-id="13d1d-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="13d1d-685">*Новый шаблон, с помощью разметки Razor и HTML5*</span><span class="sxs-lookup"><span data-stu-id="13d1d-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="13d1d-686">**Включены библиотеки JavaScript**</span><span class="sxs-lookup"><span data-stu-id="13d1d-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="13d1d-687">**jQuery**: jQuery упрощает обход документа HTML, обработка событий, анимации и взаимодействия с Ajax.</span><span class="sxs-lookup"><span data-stu-id="13d1d-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="13d1d-688">**пользовательский Интерфейс jQuery**: Эта библиотека предоставляет абстракции для взаимодействия низкого уровня, а также анимации, расширенные эффекты и мини-приложения могут изменяться темами, созданная на основе библиотеку JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="13d1d-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="13d1d-689">Вы можете узнать о jQuery и пользовательский Интерфейс jQuery в [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="13d1d-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="13d1d-690">**KnockoutJS**: шаблон по умолчанию ASP.NET MVC 4 теперь включает **KnockoutJS**, платформу JavaScript MVVM, которая позволяет создавать широкие возможности и малым временем отклика веб-приложения с использованием JavaScript и HTML.</span><span class="sxs-lookup"><span data-stu-id="13d1d-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="13d1d-691">Как и в ASP.NET MVC 3, jQuery и библиотеки пользовательского интерфейса jQuery также включаются в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="13d1d-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="13d1d-692">Можно получить дополнительные сведения о библиотеке KnockOutJS в этой связи: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="13d1d-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="13d1d-693">**Modernizr**: Эта библиотека запускается автоматически, создание сайтов, совместимые со старыми браузерами, при использовании технологий HTML5 и CSS3.</span><span class="sxs-lookup"><span data-stu-id="13d1d-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="13d1d-694">Можно получить дополнительные сведения о библиотеке Modernizr в этой связи: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="13d1d-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="13d1d-695">**SimpleMembership, включенных в решение**</span><span class="sxs-lookup"><span data-stu-id="13d1d-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="13d1d-696">SimpleMembership была разработана для замены предыдущей системы поставщика ролей ASP.NET и членства.</span><span class="sxs-lookup"><span data-stu-id="13d1d-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="13d1d-697">Он имеет много новых функций, которые упрощают для разработчиков на безопасный веб-страницы в более гибкие возможности.</span><span class="sxs-lookup"><span data-stu-id="13d1d-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="13d1d-698">Интернет-шаблоне уже задал выполнить несколько действий для интеграции SimpleMembership, например, AccountController подготовлена для использования OAuthWebSecurity (для регистрации учетной записи OAuth, имя входа, управления и др.) и веб-безопасности.</span><span class="sxs-lookup"><span data-stu-id="13d1d-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="13d1d-699">![SimpleMembership включена в решение](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership включена в решение")</span><span class="sxs-lookup"><span data-stu-id="13d1d-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="13d1d-700">*SimpleMembership включена в решение*</span><span class="sxs-lookup"><span data-stu-id="13d1d-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="13d1d-701">Дополнительные сведения о [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) в библиотеке MSDN.</span><span class="sxs-lookup"><span data-stu-id="13d1d-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="13d1d-702">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложении B: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="13d1d-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="13d1d-703">Сводка</span><span class="sxs-lookup"><span data-stu-id="13d1d-703">Summary</span></span>

<span data-ttu-id="13d1d-704">Выполнив этот практический семинар вы узнали основы ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="13d1d-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="13d1d-705">Основные элементы MVC-приложениях и их взаимодействие</span><span class="sxs-lookup"><span data-stu-id="13d1d-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="13d1d-706">Создание приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d1d-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="13d1d-707">Добавление и настройка контроллеров для обработки параметров, передаваемых через URL-адрес и строки запроса</span><span class="sxs-lookup"><span data-stu-id="13d1d-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="13d1d-708">Как добавить главную страницу макета для настройки шаблона для типичного содержимого HTML, таблицы стилей для улучшения внешнего вида и поведения и шаблона представления для отображения содержимого HTML</span><span class="sxs-lookup"><span data-stu-id="13d1d-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="13d1d-709">Как использовать шаблон ViewModel для передачи свойств в шаблоне представления для отображения динамических данных</span><span class="sxs-lookup"><span data-stu-id="13d1d-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="13d1d-710">Как использовать параметры, передаваемые контроллеров в шаблоне представления</span><span class="sxs-lookup"><span data-stu-id="13d1d-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="13d1d-711">Как добавить ссылки на страницы в приложении ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d1d-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="13d1d-712">Как добавлять и использовать динамические свойства в представлении</span><span class="sxs-lookup"><span data-stu-id="13d1d-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="13d1d-713">Усовершенствования в шаблоны проектов ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="13d1d-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="13d1d-714">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="13d1d-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="13d1d-715">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="13d1d-716">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="13d1d-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="13d1d-717">Перейдите к [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="13d1d-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="13d1d-718">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="13d1d-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="13d1d-719">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-719">Click on **Install Now**.</span></span> <span data-ttu-id="13d1d-720">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="13d1d-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="13d1d-721">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="13d1d-722">![Установка Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="13d1d-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="13d1d-723">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="13d1d-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="13d1d-724">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="13d1d-726">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="13d1d-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="13d1d-727">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-727">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="13d1d-729">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="13d1d-729">*Installation progress*</span></span>
6. <span data-ttu-id="13d1d-730">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-730">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="13d1d-732">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="13d1d-732">*Installation completed*</span></span>
7. <span data-ttu-id="13d1d-733">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="13d1d-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="13d1d-734">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="13d1d-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="13d1d-736">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="13d1d-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="13d1d-737">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="13d1d-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="13d1d-738">В этом приложении показано, как создать новый веб-сайт на портале управления Windows Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="13d1d-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="13d1d-739">Задача 1 - Создание нового веб-сайта с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="13d1d-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="13d1d-740">Перейдите к [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="13d1d-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-741">С помощью Windows Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика.</span><span class="sxs-lookup"><span data-stu-id="13d1d-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="13d1d-742">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="13d1d-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="13d1d-743">![Войдите на портал Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Войдите на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="13d1d-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="13d1d-744">*Войдите на портал управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="13d1d-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="13d1d-745">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="13d1d-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="13d1d-746">![Создание нового веб-сайта](aspnet-mvc-4-fundamentals/_static/image49.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="13d1d-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="13d1d-747">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="13d1d-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="13d1d-748">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="13d1d-749">Затем выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="13d1d-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="13d1d-750">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-751">Веб-сайта Windows Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="13d1d-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="13d1d-752">Быстрое создание позволяет развернуть завершенное веб-приложения для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="13d1d-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="13d1d-753">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="13d1d-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="13d1d-754">![Создание нового веб-сайта, с помощью быстрого создания](aspnet-mvc-4-fundamentals/_static/image50.png "создания нового веб-сайта, с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="13d1d-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="13d1d-755">*Создание нового веб-сайта, с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="13d1d-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="13d1d-756">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="13d1d-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="13d1d-757">После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="13d1d-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="13d1d-758">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="13d1d-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="13d1d-759">![Выбрав новый веб-сайт](aspnet-mvc-4-fundamentals/_static/image51.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="13d1d-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="13d1d-760">*Выбрав новый веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="13d1d-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="13d1d-761">![Веб-сайт работал](aspnet-mvc-4-fundamentals/_static/image52.png "веб-узлом")</span><span class="sxs-lookup"><span data-stu-id="13d1d-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="13d1d-762">*Веб-сайт под управлением*</span><span class="sxs-lookup"><span data-stu-id="13d1d-762">*Web site running*</span></span>
6. <span data-ttu-id="13d1d-763">Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="13d1d-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="13d1d-764">![Открытие страницы управления веб-сайт](aspnet-mvc-4-fundamentals/_static/image53.png "Открытие страницы управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="13d1d-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="13d1d-765">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="13d1d-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="13d1d-766">В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="13d1d-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-767">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайт Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="13d1d-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="13d1d-768">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации.</span><span class="sxs-lookup"><span data-stu-id="13d1d-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="13d1d-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений на веб-сайтах Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="13d1d-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="13d1d-770">![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-fundamentals/_static/image54.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="13d1d-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="13d1d-771">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="13d1d-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="13d1d-772">Скачайте файл профиля публикации в известном месте.</span><span class="sxs-lookup"><span data-stu-id="13d1d-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="13d1d-773">Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения на веб-сайтах, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d1d-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="13d1d-774">![Сохранение файла профиля публикации](aspnet-mvc-4-fundamentals/_static/image55.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="13d1d-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="13d1d-775">*Сохранение файла профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="13d1d-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="13d1d-776">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="13d1d-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="13d1d-777">Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="13d1d-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="13d1d-778">Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.</span><span class="sxs-lookup"><span data-stu-id="13d1d-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="13d1d-779">Вам потребуется сервер базы данных SQL для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="13d1d-780">Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="13d1d-781">Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="13d1d-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="13d1d-782">Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="13d1d-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="13d1d-783">Не создавайте базы данных, так как он будет создан в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="13d1d-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="13d1d-784">![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-fundamentals/_static/image56.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="13d1d-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="13d1d-785">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="13d1d-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="13d1d-786">В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="13d1d-787">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="13d1d-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Добавление IP-адрес клиента](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="13d1d-789">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="13d1d-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="13d1d-790">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="13d1d-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="13d1d-792">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="13d1d-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="13d1d-793">Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="13d1d-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="13d1d-794">Вернитесь к решению для ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="13d1d-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="13d1d-795">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="13d1d-796">![Публикация приложения](aspnet-mvc-4-fundamentals/_static/image60.png "публикации приложения")</span><span class="sxs-lookup"><span data-stu-id="13d1d-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="13d1d-797">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="13d1d-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="13d1d-798">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="13d1d-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="13d1d-799">![Импорт профиля публикации](aspnet-mvc-4-fundamentals/_static/image61.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="13d1d-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="13d1d-800">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="13d1d-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="13d1d-801">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-801">Click **Validate Connection**.</span></span> <span data-ttu-id="13d1d-802">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13d1d-803">Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.</span><span class="sxs-lookup"><span data-stu-id="13d1d-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="13d1d-804">![Проверка подключения](aspnet-mvc-4-fundamentals/_static/image62.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="13d1d-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="13d1d-805">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="13d1d-805">*Validating connection*</span></span>
4. <span data-ttu-id="13d1d-806">В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="13d1d-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="13d1d-807">![Конфигурация веб-развертывания](aspnet-mvc-4-fundamentals/_static/image63.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="13d1d-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="13d1d-808">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="13d1d-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="13d1d-809">Настройте подключение к базе данных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="13d1d-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="13d1d-810">В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="13d1d-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="13d1d-811">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="13d1d-812">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="13d1d-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="13d1d-813">Введите новое имя базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="13d1d-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="13d1d-814">![Настройка строки подключения назначения](aspnet-mvc-4-fundamentals/_static/image64.png "Настройка строка соединения с назначением")</span><span class="sxs-lookup"><span data-stu-id="13d1d-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="13d1d-815">*Настройка строки подключения назначения*</span><span class="sxs-lookup"><span data-stu-id="13d1d-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="13d1d-816">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-816">Then click **OK**.</span></span> <span data-ttu-id="13d1d-817">При появлении запроса на создание базы данных нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="13d1d-818">![Создание базы данных](aspnet-mvc-4-fundamentals/_static/image65.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="13d1d-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="13d1d-819">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="13d1d-819">*Creating the database*</span></span>
7. <span data-ttu-id="13d1d-820">В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="13d1d-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="13d1d-821">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-821">Then click **Next**.</span></span>

    <span data-ttu-id="13d1d-822">![Строка подключения, указывающий на базу данных SQL](aspnet-mvc-4-fundamentals/_static/image66.png "строку подключения, указывающий на базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="13d1d-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="13d1d-823">*Строка подключения, указывающий на базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="13d1d-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="13d1d-824">В **предварительной версии** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="13d1d-825">![Публикация веб-приложения](aspnet-mvc-4-fundamentals/_static/image67.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="13d1d-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="13d1d-826">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="13d1d-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="13d1d-827">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="13d1d-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="13d1d-828">![Приложение опубликовано в Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="13d1d-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="13d1d-829">*Приложение будет публиковаться в Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="13d1d-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="13d1d-830">Приложение c. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="13d1d-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="13d1d-831">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="13d1d-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="13d1d-832">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="13d1d-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="13d1d-833">![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-fundamentals/_static/image69.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="13d1d-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="13d1d-834">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="13d1d-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="13d1d-835">***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="13d1d-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="13d1d-836">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="13d1d-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="13d1d-837">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="13d1d-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="13d1d-838">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="13d1d-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="13d1d-839">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="13d1d-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="13d1d-840">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="13d1d-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="13d1d-841">![Начните вводить имя фрагмента](aspnet-mvc-4-fundamentals/_static/image70.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="13d1d-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="13d1d-842">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="13d1d-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="13d1d-843">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-fundamentals/_static/image71.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="13d1d-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="13d1d-844">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="13d1d-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="13d1d-845">![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-fundamentals/_static/image72.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="13d1d-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="13d1d-846">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="13d1d-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="13d1d-847">***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="13d1d-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="13d1d-848">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="13d1d-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="13d1d-849">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="13d1d-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="13d1d-850">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="13d1d-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="13d1d-851">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-fundamentals/_static/image73.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="13d1d-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="13d1d-852">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="13d1d-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="13d1d-853">![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-fundamentals/_static/image74.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="13d1d-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="13d1d-854">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="13d1d-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
