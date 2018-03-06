---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "Вспомогательные объекты ASP.NET MVC 4, форм и проверки | Документы Microsoft"
author: rick-anderson
description: "В ASP.NET MVC 4 моделей и доступа к данным в практической работе вы были Загрузка и отображение данных из базы данных. В этой лаборатории практических добавляемого..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 243db3708ac4311d423c4c137f503f072f5553e6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="d6a2a-104">Вспомогательные объекты ASP.NET MVC 4, форм и проверки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="d6a2a-105">По [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d6a2a-106">Загрузите комплект учебных материалов лагеря Web</span><span class="sxs-lookup"><span data-stu-id="d6a2a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="d6a2a-107">В **ASP.NET MVC 4 моделей и доступа к данным** Практическое лабораторное занятие, уже Загрузка и отображение данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="d6a2a-108">В этой лаборатории практических добавляемого **Music Store** приложения возможности изменения этих данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="d6a2a-109">С этой целью в сначала создается контроллер, который будет поддерживать альбомов действия создания, чтения, обновления и удаления (CRUD).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="d6a2a-110">Будет сформировать шаблон представление Index преимуществами ASP.NET MVC функции формирования шаблонов для отображения свойств альбомов таблицы HTML.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="d6a2a-111">Чтобы улучшить представление, добавлении пользовательских вспомогательный метод HTML, который будет выполнять усечение длинные описания.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="d6a2a-112">После этого вы добавите, изменение и создание представлений, которые позволят изменять альбомы в базе данных с помощью элементов формы, например раскрывающиеся меню.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="d6a2a-113">Наконец позволит пользователям удалять альбом и также можно будет предотвратить ввод неверных данных путем проверки их входных данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="d6a2a-114">Это Практическое лабораторное занятие предполагает иметь базовые знания об **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="d6a2a-115">Если вы не использовали **ASP.NET MVC** раньше, мы рекомендуем перейти **ASP.NET MVC основы** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="d6a2a-116">Эта лаборатория рассматриваются улучшения и новые функции, описанные ранее путем применения незначительные изменения примера веб-приложения в папке исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a2a-117">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="d6a2a-118">Проект для этой лаборатории доступен на [ASP.NET MVC 4 помощников, форм и проверки](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d6a2a-119">Цели</span><span class="sxs-lookup"><span data-stu-id="d6a2a-119">Objectives</span></span>

<span data-ttu-id="d6a2a-120">В этой лаборатории практических вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="d6a2a-121">Создание контроллера для поддержки операций CRUD</span><span class="sxs-lookup"><span data-stu-id="d6a2a-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="d6a2a-122">Создания индекса представления для отображения свойств сущности таблицы HTML</span><span class="sxs-lookup"><span data-stu-id="d6a2a-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="d6a2a-123">Добавление пользовательских вспомогательный метод HTML</span><span class="sxs-lookup"><span data-stu-id="d6a2a-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="d6a2a-124">Создание и настройка Изменение представления</span><span class="sxs-lookup"><span data-stu-id="d6a2a-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="d6a2a-125">Различия между методами действий, реагирующие на HTTP-GET или HTTP POST вызовов</span><span class="sxs-lookup"><span data-stu-id="d6a2a-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="d6a2a-126">Добавление и настройка Create View</span><span class="sxs-lookup"><span data-stu-id="d6a2a-126">Add and customize a Create View</span></span>
- <span data-ttu-id="d6a2a-127">Дескриптор удаления сущности</span><span class="sxs-lookup"><span data-stu-id="d6a2a-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="d6a2a-128">Проверка введенных пользователем данных</span><span class="sxs-lookup"><span data-stu-id="d6a2a-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d6a2a-129">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d6a2a-129">Prerequisites</span></span>

<span data-ttu-id="d6a2a-130">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="d6a2a-131">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d6a2a-132">Установка</span><span class="sxs-lookup"><span data-stu-id="d6a2a-132">Setup</span></span>

<span data-ttu-id="d6a2a-133">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="d6a2a-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="d6a2a-134">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="d6a2a-135">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="d6a2a-136">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot;[с помощью фрагментов кода в приложении б](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d6a2a-137">Упражнения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-137">Exercises</span></span>

<span data-ttu-id="d6a2a-138">Следующие упражнения составляют это Практическое лабораторное занятие:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="d6a2a-139">Создание контроллера директор магазина и его представление индекса</span><span class="sxs-lookup"><span data-stu-id="d6a2a-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="d6a2a-140">Добавление вспомогательный метод HTML</span><span class="sxs-lookup"><span data-stu-id="d6a2a-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="d6a2a-141">Создание представления изменения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="d6a2a-142">Добавление представления создания</span><span class="sxs-lookup"><span data-stu-id="d6a2a-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="d6a2a-143">Обработка удаления</span><span class="sxs-lookup"><span data-stu-id="d6a2a-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="d6a2a-144">Добавление проверки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="d6a2a-145">С помощью jQuery ненавязчивого на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="d6a2a-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="d6a2a-146">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d6a2a-147">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="d6a2a-148">Предполагаемое время для выполнения этого занятия: **60 минут**</span><span class="sxs-lookup"><span data-stu-id="d6a2a-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="d6a2a-149">Упражнение 1: Создание контроллера директор магазина и его представление индекса</span><span class="sxs-lookup"><span data-stu-id="d6a2a-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="d6a2a-150">В этом упражнении вы узнаете, как создать новый контроллер для поддержки операций CRUD, настроить его метод действия индекса для получения списка альбомов из базы данных и наконец создается шаблон представление Index преимуществами формирование шаблонов ASP.NET MVC средство для отображения свойств альбомов таблицы HTML.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="d6a2a-151">Задача 1. Создание StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="d6a2a-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="d6a2a-152">В этой задаче вы создадите новый контроллер с именем **StoreManagerController** для поддержки операций CRUD.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="d6a2a-153">Откройте **начать** решений, расположенный **источника или сервера Ex1-CreatingTheStoreManagerController/Begin/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="d6a2a-154">Необходимо загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-155">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-156">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-157">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-158">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-159">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-160">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-161">Добавление нового устройства.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-161">Add a new controller.</span></span> <span data-ttu-id="d6a2a-162">Для этого щелкните правой кнопкой мыши **контроллеров** папки в обозревателе решений выберите **добавить** и затем **контроллера** команды.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="d6a2a-163">Изменение **контроллера** **имя** для **StoreManagerController** и убедитесь, что параметр **контроллер MVC с действиями пустой чтения и записи**выбран.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="d6a2a-164">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-164">Click **Add**.</span></span>

    <span data-ttu-id="d6a2a-165">![Диалоговое окно Добавить контроллер](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "диалоговое окно Добавить контроллер")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="d6a2a-166">*Диалоговое окно добавления контроллера*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="d6a2a-167">Создается новый класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-167">A new Controller class is generated.</span></span> <span data-ttu-id="d6a2a-168">Поскольку вы указываете для добавления действий для чтения и записи, методы-заглушки для тех, типовых операций CRUD создаются с комментарии TODO, заполнено, запроса содержит определенную логику приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="d6a2a-169">Задача 2 — Настройка StoreManager индекса</span><span class="sxs-lookup"><span data-stu-id="d6a2a-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="d6a2a-170">В этой задаче будет выполнена настройка метода действия индекса StoreManager возвратить представление со списком Альбомы из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="d6a2a-171">В классе StoreManagerController, добавьте следующий *с помощью* директивы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="d6a2a-172">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - сервера Ex1 с помощью MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="d6a2a-173">Добавление полей для **StoreManagerController** для хранения экземпляра **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="d6a2a-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="d6a2a-174">(Фрагмент - кода *вспомогательные объекты ASP.NET MVC 4, форм и проверки - MusicStoreEntities сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="d6a2a-175">Реализуйте действие StoreManagerController индекса, чтобы вернуть представление со списком альбомов.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="d6a2a-176">Логика действия контроллера будет очень похожа на действие StoreController индекса, записанных ранее.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="d6a2a-177">Получение всех альбомы, включая сведения о жанр и исполнитель для отображения с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="d6a2a-178">(Фрагмент - кода *вспомогательные объекты ASP.NET MVC 4, форм и проверки - индекс StoreManagerController сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="d6a2a-179">Задача 3. Создание индекса представления</span><span class="sxs-lookup"><span data-stu-id="d6a2a-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="d6a2a-180">В этой задаче вы создадите представление Index шаблон для отображения списка альбомы, возвращенных **StoreManager** контроллера.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="d6a2a-181">Перед созданием нового шаблона представления, при построении проекта, чтобы **добавить диалоговое окно представления** знает о **альбом** класс использовать.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="d6a2a-182">Выберите **сборки | Построение MvcMusicStore** для сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="d6a2a-183">Щелкните правой кнопкой мыши внутри **индекс** метода действия и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="d6a2a-184">Данная команда откроет **добавить представление** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="d6a2a-185">![Добавить представление](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "добавить представление")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="d6a2a-186">*Добавление представления из метода индекса*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="d6a2a-187">В диалоговом окне Добавить представление, убедитесь, что имя представления **индекса**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="d6a2a-188">Выберите **создать строго типизированное представление** и выбрать **альбом (MvcMusicStore.Models)** из **класс модели** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="d6a2a-189">Выберите **списка** из **шаблон формирования** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="d6a2a-190">Оставить **обработчик представлений** для **Razor** и другие поля, их значения по умолчанию значение и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="d6a2a-191">![Добавление представления индекс](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Добавление индекса представления")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="d6a2a-192">*Добавление индекса представления*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="d6a2a-193">Задача 4. Настройка формирования шаблонов индекса представления</span><span class="sxs-lookup"><span data-stu-id="d6a2a-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="d6a2a-194">В этой задаче можно отрегулировать простой шаблон представления, созданных с помощью ASP.NET MVC функции формирования шаблонов для его отображения нужные поля.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a2a-195">**Формирование шаблонов** поддержки в ASP.NET MVC создает простой шаблон представления, который содержит перечень всех полей в модели альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="d6a2a-196">**Формирование шаблонов** предоставляет быстрый способ, чтобы приступить к работе в строго типизированном представлении: вместо того, чтобы создать шаблон представления вручную, формирование шаблонов быстро создает шаблон по умолчанию и затем изменить созданный код.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="d6a2a-197">Просмотрите код, созданный.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-197">Review the code created.</span></span> <span data-ttu-id="d6a2a-198">Созданный список полей будет входить следующие таблицы HTML, **формирование шаблонов** используется для отображения табличных данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="d6a2a-199">Замените  **&lt;таблицы&gt;**  код на следующий код, чтобы отобразить только **жанр**, **исполнителя**, **название альбома**, и **цены** поля.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="d6a2a-200">Эта функция удаляет **AlbumId** и **URL-адрес рисунка альбом** столбцов.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="d6a2a-201">Кроме того, он изменяет GenreId и ArtistId столбцов для отображения их свойств связанного класса **Artist.Name** и **Genre.Name**и удаляет **сведения** ссылку.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="d6a2a-202">Измените следующие описания.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="d6a2a-203">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="d6a2a-204">В этой задаче вы проверите, **StoreManager** **индекс** шаблон просмотра отображается список альбомов согласно конструктора из приведенных выше шагов.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="d6a2a-205">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-206">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-206">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-207">Изменить URL-адрес **/StoreManager** чтобы убедиться, что отображается список альбомы, показывающая их **заголовок**, **исполнителя** и **жанр**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="d6a2a-208">![Просмотр списка альбомы](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "просмотра списка альбомов")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="d6a2a-209">*Просмотр списка альбомов*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="d6a2a-210">Упражнение 2: Добавление вспомогательный метод HTML</span><span class="sxs-lookup"><span data-stu-id="d6a2a-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="d6a2a-211">Страницы индекса StoreManager есть одна возможная проблема: заголовок и исполнитель свойства могут быть достаточно большим, чтобы сбросить форматирование таблицы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="d6a2a-212">В этом упражнении вы узнаете, как добавить пользовательские вспомогательный метод HTML для усечения данным текстом.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="d6a2a-213">На следующем рисунке вы увидите, как изменить формат из-за длины текста, при использовании браузера небольшой размер.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="d6a2a-214">![Просмотр списка альбомов с не усекается текст](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "не просмотра списка альбомов с усеченный текст")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="d6a2a-215">*Просмотр списка альбомов с не усекается текст*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="d6a2a-216">Задача 1 - расширение вспомогательный метод HTML</span><span class="sxs-lookup"><span data-stu-id="d6a2a-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="d6a2a-217">В этой задаче будет добавлен новый метод **Truncate** для **HTML** объекту, предоставленному в представлениях MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="d6a2a-218">Чтобы сделать это, реализовать **метод расширения** на встроенные **System.Web.Mvc.HtmlHelper** класс, предоставляемый ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a2a-219">Чтобы узнать подробнее об **методы расширения**, можно найти в этой статье msdn.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="d6a2a-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="d6a2a-221">Откройте **начать** решений, расположенный **источника/Ex2-AddingAnHTMLHelper/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="d6a2a-222">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d6a2a-223">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-224">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-225">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-226">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-227">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-228">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-229">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-230">Откройте представление индекса элемента StoreManager.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="d6a2a-231">Для этого в обозревателе решений разверните **представления** папки, а затем **StoreManager** и откройте **Index.cshtml** файла.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="d6a2a-232">Добавьте следующий код ниже  **@model**  директивы для определения **Truncate** вспомогательный метод.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-232">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="d6a2a-233">Задача 2 - усечение текста на странице</span><span class="sxs-lookup"><span data-stu-id="d6a2a-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="d6a2a-234">В этой задаче будет использоваться **Truncate** способ усечения текста в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="d6a2a-235">Откройте представление индекса элемента StoreManager.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="d6a2a-236">Для этого в обозревателе решений разверните **представления** папки, а затем **StoreManager** и откройте **Index.cshtml** файла.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="d6a2a-237">Замените строки, содержащие **исполнитель** и альбома **заголовка**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="d6a2a-238">Чтобы сделать это, замените следующие строки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="d6a2a-239">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="d6a2a-240">В этой задаче вы проверите, **StoreManager** **индекс** Просмотр шаблона усекает название альбома, а также имя исполнителя.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="d6a2a-241">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-242">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-242">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-243">Изменить URL-адрес **/StoreManager** для проверки этой долго текстов на **заголовок** и **исполнителя** столбца, усекаются.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="d6a2a-244">![Усечение имен заголовков и исполнители](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "усечены имена заголовков и исполнители")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="d6a2a-245">*Усеченный заголовки и имена исполнителей*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="d6a2a-246">Упражнение 3: Создание представления изменения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="d6a2a-247">В этом упражнении вы узнаете, как создать форму, чтобы разрешить диспетчеров хранилища для редактирования альбом.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="d6a2a-248">Они смогут просматривать **/StoreManager/Edit/id** URL-адрес (**идентификатор** выполняется альбома, чтобы изменить уникальный идентификатор), таким образом выполнения вызова HTTP-GET на сервер.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="d6a2a-249">Метод действия контроллера Edit будет извлекать соответствующие альбома из базы данных, создайте **StoreManagerViewModel** объекта для инкапсуляции она (а также список исполнителей и жанров) и затем передавать его шаблона представления для отображать страницу HTML для пользователя.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="d6a2a-250">Эта страница будет содержать  **&lt;формы&gt;**  элемент с текстовые поля и раскрывающиеся меню для редактирования свойства альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="d6a2a-251">Когда пользователь обновляет значения альбом форму и выбирает **Сохранить** кнопки изменения передаются через HTTP POST обратный вызов **/StoreManager/Edit/id**. Несмотря на то, что URL-адрес остается тем же, как и последнего вызова метода, ASP.NET MVC определяет, что это время она HTTP POST и поэтому выполняет другой метод действия редактирования (один декорированных **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="d6a2a-252">Задача 1 — реализация метода HTTP GET изменить действие</span><span class="sxs-lookup"><span data-stu-id="d6a2a-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="d6a2a-253">В этой задаче будет реализовать версию HTTP-GET метода действия редактирования для получения соответствующих альбома из базы данных, а также список всех жанров и исполнителей.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="d6a2a-254">Он будет запаковать эти данные в **StoreManagerViewModel** объект, определенный на предыдущем шаге, который затем передается в представление шаблона для отображения ответа с.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="d6a2a-255">Откройте **начать** решений, расположенный **источника/Ex3-CreatingTheEditView/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="d6a2a-256">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d6a2a-257">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-258">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-259">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-260">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-261">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-262">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-263">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-264">Откройте **StoreManagerController** класса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="d6a2a-265">Для этого разверните **контроллеров** папку и дважды щелкните **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="d6a2a-266">Замените **изменить HTTP-GET** следующий код, чтобы получить соответствующий метод действия **альбом** а также **жанров** и **исполнители**перечислены.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="d6a2a-267">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - Ex3 StoreManagerController HTTP-GET изменить действие*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-268">Вы используете **System.Web.Mvc** **SelectList** исполнителей и жанров вместо **System.Collections.Generic** списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="d6a2a-269">**SelectList** очистки способ заполнения раскрывающихся списках HTML и управления тем, как текущее выделение.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="d6a2a-270">При создании экземпляра и более поздних версий настройки этих объектов ViewModel действия контроллера будет того, чтобы изменить формы сценарий очистки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="d6a2a-271">Задача 2 - Создание представления изменения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="d6a2a-272">В этой задаче вы создадите представление изменить шаблон, который позднее будет отображать свойства альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="d6a2a-273">Создание представления редактирования.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-273">Create the Edit View.</span></span> <span data-ttu-id="d6a2a-274">Для этого щелкните правой кнопкой мыши внутри **изменить** метода действия и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="d6a2a-275">В диалоговом окне Добавить представление, убедитесь, что имя представления **изменить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="d6a2a-276">Проверьте **создать строго типизированное представление** флажок и выбрать **альбом (MvcMusicStore.Models)** из **просматривать класс данных** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="d6a2a-277">Выберите **изменить** из **шаблон формирования** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="d6a2a-278">Оставьте значения по умолчанию другие поля и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="d6a2a-279">![Добавление представления изменения](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Добавление представления изменения")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="d6a2a-280">*Добавление представления изменения*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="d6a2a-281">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="d6a2a-282">В этой задаче вы проверите, **StoreManager** **изменить** Просмотр страницы отображаются значения свойств для альбома, переданный в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="d6a2a-283">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-284">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-284">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-285">Изменить URL-адрес **/StoreManager/Edit/1** для убедитесь, что отображаются значения свойств для переданного альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="d6a2a-286">![Просмотр представления изменения альбома](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "просмотра альбома представления изменения")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="d6a2a-287">*Просмотр представления изменения альбома*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="d6a2a-288">Задача 4. Реализация раскрывающихся списков на шаблоне альбом редактора</span><span class="sxs-lookup"><span data-stu-id="d6a2a-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="d6a2a-289">В этой задаче вы добавите раскрывающийся список для представления шаблон, созданный в последней задаче, чтобы пользователь мог выбрать из списка исполнителей и жанров.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="d6a2a-290">Заменить все **альбом** fieldset код следующим:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-291">**Html.DropDownList** вспомогательный был добавлен к просмотру раскрывающийся список для выбора исполнителей и жанров.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="d6a2a-292">Параметры, передаваемые **Html.DropDownList** являются:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="d6a2a-293">Имя поля формы (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="d6a2a-294">**SelectList** значений для раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="d6a2a-295">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="d6a2a-296">В этой задаче вы проверите, **StoreManager** **изменить** представление страница отображает раскрывающийся список, вместо исполнителя и идентификатор жанр текстовые поля.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="d6a2a-297">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-298">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-298">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-299">Изменить URL-адрес **/StoreManager/Edit/1** для убедитесь, что он отображает раскрывающийся список, вместо исполнителя и идентификатор жанр текстовые поля.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="d6a2a-300">![Просмотр альбома Изменение представления с раскрывающимся спискам](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "изменить представление просмотра альбом с раскрывающиеся списки")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="d6a2a-301">*Представление изменения обзора альбома, это время с раскрывающимися списками*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="d6a2a-302">Задача 6. реализация метода HTTP POST изменить действие</span><span class="sxs-lookup"><span data-stu-id="d6a2a-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="d6a2a-303">Теперь, когда изменение представления отображаются должным образом, необходимо реализовать метод HTTP POST изменить действие, чтобы сохранить изменения, внесенные в альбоме.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="d6a2a-304">Закройте браузер, если необходимо, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="d6a2a-305">Откройте **StoreManagerController** из **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="d6a2a-306">Замените **изменить HTTP POST** код метода действия со следующими (Обратите внимание, что метод, который должен быть заменен перегруженную версию, которая принимает два параметра):</span><span class="sxs-lookup"><span data-stu-id="d6a2a-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="d6a2a-307">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - Ex3 StoreManagerController HTTP POST изменить действие*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-308">Этот метод будет выполняться, когда пользователь щелкает **Сохранить** кнопку представления и выполняет HTTP-POST значений формы на сервер для сохранения их в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="d6a2a-309">Декоратор **[HttpPost]** указывает, что метод следует использовать для этих сценариев HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="d6a2a-310">Этот метод принимает **альбом** объекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-310">The method takes an **Album** object.</span></span> <span data-ttu-id="d6a2a-311">ASP.NET MVC автоматически создаст объект альбома из отправленной &lt;формы&gt; значения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="d6a2a-312">Метод выполнит следующие действия:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="d6a2a-313">Если модель допустима:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="d6a2a-314">Обновите запись альбом в контексте, чтобы пометить его как измененный объект.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="d6a2a-315">Сохранить изменения и перенаправления в представлении индекса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="d6a2a-316">Если модель не является допустимым, он заполняет ViewBag с **GenreId** и **ArtistId**, он возвратит представление с полученного объекта альбом разрешения пользователю выполнять любые необходимые обновления.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="d6a2a-317">Задача 7. выполнение приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="d6a2a-318">В этой задаче вы проверите, **StoreManager редактирование** страница представления фактически сохраняет обновленные данные альбом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="d6a2a-319">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-320">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-320">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-321">Изменить URL-адрес **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="d6a2a-322">Изменить название альбома для **нагрузки** и выберите команду **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="d6a2a-323">Убедитесь, что название альбома фактически изменены в списке альбомов.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="d6a2a-324">![Обновление альбом](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "обновление альбом")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="d6a2a-325">*Обновление альбом*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="d6a2a-326">Упражнение 4: Добавление представления создания</span><span class="sxs-lookup"><span data-stu-id="d6a2a-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="d6a2a-327">Теперь, когда **StoreManagerController** поддерживает **изменить** возможность в этом упражнении вы узнаете, как добавление шаблона Create View, позволяющий хранить диспетчеры Добавление новых в приложение.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="d6a2a-328">Как это было сделано с помощью функции редактирования, реализуется с помощью двух отдельных методов в сценарий создание **StoreManagerController** класса:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="d6a2a-329">Первый метод действия будет отображать пустую форму, когда хранилище диспетчеры сначала посетите **/StoreManager/Create** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="d6a2a-330">Второй метод действия будет обрабатывать сценарий которых диспетчер хранилища выбирает **Сохранить** кнопку в форме и отправляет обратно значения **/StoreManager/Create** URL-адрес как запрос HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="d6a2a-331">Задача 1 — реализация метода действия создания HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="d6a2a-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="d6a2a-332">В этой задаче будет реализована версия HTTP-GET метод действия Create, чтобы получить список всех жанров и исполнителей, упаковать эти данные в **StoreManagerViewModel** объекта, который затем можно передать шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="d6a2a-333">Откройте **начать** решений, расположенный **источника/Ex4-AddingACreateView/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="d6a2a-334">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d6a2a-335">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-336">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-337">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-338">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-339">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-340">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-341">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-342">Откройте **StoreManagerController** класса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="d6a2a-343">Для этого разверните **контроллеров** папку и дважды щелкните **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="d6a2a-344">Замените **создать** код метода действия в следующем:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="d6a2a-345">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - действие создания HTTP-GET StoreManagerController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="d6a2a-346">Задача 2 - Добавление представления создания</span><span class="sxs-lookup"><span data-stu-id="d6a2a-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="d6a2a-347">В этой задаче вы добавите шаблон Create View, который будет отображать форма нового альбома (пустой).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="d6a2a-348">Щелкните правой кнопкой мыши внутри **создать** метода действия и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="d6a2a-349">Откроется диалоговое окно добавления представления.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="d6a2a-350">В диалоговом окне Добавить представление, убедитесь, что имя представления **создать**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="d6a2a-351">Выберите **создать строго типизированное представление** и выберите **альбом (MvcMusicStore.Models)** из **класс модели** раскрывающегося списка и **создать** из **шаблон формирования** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="d6a2a-352">Оставьте значения по умолчанию другие поля и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="d6a2a-353">![Добавление представления создания](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Добавление a создать view.png")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="d6a2a-354">*Добавление представления создания*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="d6a2a-355">Обновление **GenreId** и **ArtistId** поля для использования раскрывающегося списка, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="d6a2a-356">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="d6a2a-357">В этой задаче вы проверите, **StoreManager** **создать** страница «просмотр» отображает пустую форму альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="d6a2a-358">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-359">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-359">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-360">Изменить URL-адрес **/StoreManager/создание**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="d6a2a-361">Убедитесь, что для заполнения свойства нового альбома отображается пустую форму.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="d6a2a-362">![Создание представления с пустую форму](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View с пустой формы")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="d6a2a-363">*Создание представления с пустой формы*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="d6a2a-364">Задача 4. реализация HTTP POST Создание метода действия</span><span class="sxs-lookup"><span data-stu-id="d6a2a-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="d6a2a-365">В этой задаче будет реализована версия HTTP POST метода действия Create, который будет вызываться, когда пользователь щелкает **Сохранить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="d6a2a-366">Метод следует сохранить новый альбом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="d6a2a-367">Закройте браузер, если необходимо, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="d6a2a-368">Откройте **StoreManagerController** класса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="d6a2a-369">Для этого разверните **контроллеров** папку и дважды щелкните **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="d6a2a-370">Замените **HTTP POST создать** код метода действия в следующем:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="d6a2a-371">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - Ex4 StoreManagerController HTTP - POST действия Create*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-372">Действие создания довольно аналогично предыдущему методу действия редактирования, но вместо задания объект как измененный, оно добавляется в контекст.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="d6a2a-373">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="d6a2a-374">В этой задаче вы проверите, **StoreManager создания** страница представления позволяет создавать нового альбома и выполняет перенаправление к представлению StoreManager индекса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="d6a2a-375">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-376">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-376">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-377">Изменить URL-адрес **/StoreManager/создание**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="d6a2a-378">Заполните все поля формы с данными для нового альбома, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="d6a2a-379">![Создание альбом](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "создания альбома")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="d6a2a-380">*Создание альбом*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-380">*Creating an Album*</span></span>
3. <span data-ttu-id="d6a2a-381">Убедитесь, что происходит перенаправление представление Index StoreManager, включающий только что создали нового альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="d6a2a-382">![Создан новый альбом](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "создания нового альбома")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="d6a2a-383">*Создан новый альбом*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="d6a2a-384">Упражнение 5: Удаление обработки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="d6a2a-385">Возможность удаления альбомы еще не реализован.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="d6a2a-386">Это то, что в этом упражнении будет примерно.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-386">This is what this exercise will be about.</span></span> <span data-ttu-id="d6a2a-387">Как и раньше, будет реализовать сценарий удаления, с помощью двух отдельных методов в **StoreManagerController** класса:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="d6a2a-388">Первый метод действия будет отображаться форма подтверждения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="d6a2a-389">Второй метод действия будет обрабатывать отправки формы</span><span class="sxs-lookup"><span data-stu-id="d6a2a-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="d6a2a-390">Задача 1 - реализации метод действия Delete HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="d6a2a-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="d6a2a-391">В этой задаче будет реализована версия HTTP-GET метод действия Delete, чтобы получить сведения о диске.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="d6a2a-392">Откройте **начать** решений, расположенный **источника/Ex5-HandlingDeletion/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="d6a2a-393">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d6a2a-394">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-395">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-396">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-397">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-398">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-399">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-400">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-401">Откройте **StoreManagerController** класса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="d6a2a-402">Для этого разверните **контроллеров** папку и дважды щелкните **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="d6a2a-403">Действие Delete контроллера идентичен предыдущее действие контроллера сведения о хранилище: он запрашивает **альбом** объекта из базы данных с помощью **идентификатор** в URL-адрес и возвращает соответствующие **представление**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="d6a2a-404">Для этого замените HTTP-GET **удалить** действие метода код следующим:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="d6a2a-405">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - Ex5 обработки удаления HTTP-GET действие удаления*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="d6a2a-406">Щелкните правой кнопкой мыши внутри **удаление** метода действия и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="d6a2a-407">Откроется диалоговое окно добавления представления.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="d6a2a-408">В диалоговом окне Добавить представление, убедитесь, что имя представления **удалить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="d6a2a-409">Выберите **создать строго типизированное представление** и выберите **альбом (MvcMusicStore.Models)** из **класс модели** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="d6a2a-410">Выберите **удаление** из **шаблон формирования** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="d6a2a-411">Оставьте значения по умолчанию другие поля и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="d6a2a-412">![Добавление представления удаления](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Добавление представления удаления")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="d6a2a-413">*Добавление представления удаления*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="d6a2a-414">Удаление шаблона отображаются все поля из модели.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="d6a2a-415">Будут показаны только название альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-415">You will show only the album's title.</span></span> <span data-ttu-id="d6a2a-416">Для этого замените содержимое представления с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="d6a2a-417">Задача 2 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="d6a2a-418">В этой задаче вы проверите, **StoreManager** **удалить** страница «просмотр» отображает форму подтверждения удаления.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="d6a2a-419">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-420">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-420">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-421">Изменить URL-адрес **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="d6a2a-422">Выберите один альбома, щелкнув кнопку **удаление** и убедитесь, что новое представление будет загружен.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="d6a2a-423">![Удаление альбом](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "удаление альбом")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="d6a2a-424">*Удаление альбом*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="d6a2a-425">Задача 3 — реализация метода действия Delete HTTP POST</span><span class="sxs-lookup"><span data-stu-id="d6a2a-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="d6a2a-426">В этой задаче будет реализована версия HTTP POST метод действия Delete, который будет вызываться, когда пользователь щелкает **удалить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="d6a2a-427">Метод следует удалить альбом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="d6a2a-428">Закройте браузер, если необходимо, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="d6a2a-429">Откройте **StoreManagerController** класса.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="d6a2a-430">Для этого разверните **контроллеров** папку и дважды щелкните **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="d6a2a-431">Замените **удалить HTTP POST** действие метода код следующим:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="d6a2a-432">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверки - Ex5 обработки удаления HTTP POST действие удаления*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="d6a2a-433">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="d6a2a-434">В этой задаче вы проверите, **StoreManager Delete** страница представления можно удалить альбом и выполняет перенаправление StoreManager представление Index.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="d6a2a-435">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-436">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-436">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-437">Изменить URL-адрес **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="d6a2a-438">Выберите один альбома, щелкнув кнопку **удалить.**</span><span class="sxs-lookup"><span data-stu-id="d6a2a-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="d6a2a-439">Подтвердите удаление, нажав кнопку **удалить** кнопки:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="d6a2a-440">![Удаление альбом](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "удаление альбом")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="d6a2a-441">*Удаление альбом*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="d6a2a-442">Убедитесь, что альбом был удален, так как он не отображается в **индекс** страницы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="d6a2a-443">Упражнение 6: Добавление проверки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="d6a2a-444">В настоящее время создание и изменение форм, которые выполнены не выполняйте никаких проверок.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="d6a2a-445">Если пользователь покидает обязательное поле пустое или тип буквы в поле «Цена», первой ошибки, вы получите будет из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="d6a2a-446">Проверку можно добавить в приложение путем добавления заметок к данным в класс модели.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="d6a2a-447">Разрешить заметок к данным описания правила нужные, применяемые для свойств модели и ASP.NET MVC берет на себя применения и отображение соответствующего сообщения для пользователей.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="d6a2a-448">Задача 1. Добавление заметок к данным</span><span class="sxs-lookup"><span data-stu-id="d6a2a-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="d6a2a-449">В этой задаче вы добавите заметок к данным в альбоме модель, которая сделает странице Создание и изменение отображения сообщений о проверке, когда это необходимо.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="d6a2a-450">Для простой класс модели, Добавление заметки к данным только осуществляется путем добавления **с помощью** инструкции для **System.ComponentModel.DataAnnotation**, помещаться **[обязательно]**атрибута на соответствующие свойства.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="d6a2a-451">Следующий пример сделает **имя** свойство обязательное поле в представлении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="d6a2a-452">Это немного сложнее, в случаях, таких как приложения, когда создается модель EDM.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="d6a2a-453">При добавлении заметок к данным непосредственно классы модели, они будут перезаписаны при обновлении модели из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="d6a2a-454">Вместо этого можно сделать использование метаданных разделяемые классы, которые связаны с моделью и будет существовать для хранения аннотации классов, использующих **[MetadataType]** атрибута.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="d6a2a-455">Откройте **начать** решений, расположенный **источника/Ex6-AddingValidation/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="d6a2a-456">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d6a2a-457">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-458">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-459">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-460">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-461">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-462">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-463">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-464">Откройте **Album.cs** из **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="d6a2a-465">Замените **Album.cs** содержимого с помощью выделенного кода, чтобы он выглядел следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-466">Строка **[DisplayFormat(ConvertEmptyStringToNull=false)]** указывает, что пустые строки из модели не будут преобразовываться в значение null, когда в источнике данных обновляется поле данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="d6a2a-467">Этот параметр позволит избежать исключения, когда платформа Entity Framework присваивает значения null модели перед заметки данных проверяет поля.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="d6a2a-468">(Фрагмент - кода *ASP.NET MVC 4 помощников, форм и проверка - разделяемого класса метаданных альбом Ex6*)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-469">Это **альбом** содержит разделяемый класс **MetadataType** атрибут, который указывает **AlbumMetaData** класс для заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="d6a2a-470">Ниже приведено несколько атрибуты заметки к данным, которые вы используете для создания заметок к модели альбом:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="d6a2a-471">Требуется - указывает, что свойство является обязательным полем</span><span class="sxs-lookup"><span data-stu-id="d6a2a-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="d6a2a-472">DisplayName — определяет текст, используемый на поля формы и сообщений о проверке</span><span class="sxs-lookup"><span data-stu-id="d6a2a-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="d6a2a-473">DisplayFormat - указывает порядок отображения и форматирования полей данных.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="d6a2a-474">Максимальная длина строкового поля определяет StringLength-</span><span class="sxs-lookup"><span data-stu-id="d6a2a-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="d6a2a-475">Диапазон — обеспечивает минимальное и максимальное значение для числового поля</span><span class="sxs-lookup"><span data-stu-id="d6a2a-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="d6a2a-476">ScaffoldColumn - позволяет скрывать поля из редактор форм</span><span class="sxs-lookup"><span data-stu-id="d6a2a-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="d6a2a-477">Задача 2 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="d6a2a-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="d6a2a-478">В этой задаче вы проверите проверки поля, создание и редактирование страниц с помощью отображаемые имена, выбранным в последней задаче.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="d6a2a-479">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="d6a2a-480">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-480">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-481">Изменить URL-адрес **/StoreManager/создание**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="d6a2a-482">Убедитесь, что отображаемые имена соответствуют именам в разделяемом классе (как **URL-адрес рисунка альбом** вместо **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="d6a2a-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="d6a2a-483">Нажмите кнопку **создать**, без заполнения формы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="d6a2a-484">Убедитесь, что возвращены соответствующих сообщений проверки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="d6a2a-485">![Проверка полей на странице Создать](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "проверки поля на странице «Создание»")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="d6a2a-486">*Проверенные поля на странице «Создание»*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="d6a2a-487">Убедитесь, что то же происходит в **изменить** страницы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="d6a2a-488">Изменить URL-адрес **/StoreManager/Edit/1** и убедитесь, что отображаемые имена соответствуют именам в разделяемом классе (как **URL-адрес рисунка альбом** вместо **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="d6a2a-489">Пустой **заголовок** и **цены** поля и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="d6a2a-490">Убедитесь, что возвращены соответствующих сообщений проверки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-490">Verify that you get the corresponding validation messages.</span></span>

    ![Проверенные поля на странице «изменить»](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="d6a2a-492">*Проверенные поля на странице «изменить»*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="d6a2a-493">Упражнение 7: Использование ненавязчивого jQuery на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="d6a2a-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="d6a2a-494">В этом упражнении вы узнаете, как включить MVC 4 ненавязчивой проверки jQuery на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a2a-495">Ненавязчивого jQuery использует префикс данных ajax JavaScript для вызова методов действия на сервере, а не мешая основной работе выпускающей встроенные клиентские скрипты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="d6a2a-496">Задача 1. запуск приложения до включения ненавязчивого jQuery</span><span class="sxs-lookup"><span data-stu-id="d6a2a-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="d6a2a-497">В этой задаче будет запускаться приложение перед включением jQuery, чтобы сравнить обе проверки модели.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="d6a2a-498">Откройте **начать** решений, расположенный **источника/Ex7-UnobtrusivejQueryValidation/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="d6a2a-499">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="d6a2a-500">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d6a2a-501">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="d6a2a-502">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="d6a2a-503">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d6a2a-504">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d6a2a-505">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d6a2a-506">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d6a2a-507">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="d6a2a-508">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-508">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-509">Обзор **/StoreManager/Create** и нажмите кнопку **создать** без заполнения форму, чтобы убедиться, чтобы получить сообщения проверки:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="d6a2a-510">![Проверка клиента отключена](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "проверка клиента отключена")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="d6a2a-511">*Проверка клиента отключена*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="d6a2a-512">В браузере откройте исходный код HTML:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="d6a2a-513">Задача 2 - Включение ненавязчивой клиентской проверки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="d6a2a-514">В этой задаче будет включена jQuery **ненавязчивая клиентская проверка** из **Web.config** файл, который по умолчанию, значение false во всех новых проектах ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="d6a2a-515">Кроме того вы добавляет необходимые скрипты ссылки вносить jQuery рабочих ненавязчивой клиентской проверки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="d6a2a-516">Откройте **Web.Config** файл в корне проекта и убедитесь, что **ClientValidationEnabled** и **UnobtrusiveJavaScriptEnabled** ключи имеют значение **true**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-517">Можно также включить проверку клиента, код в Global.asax.cs, чтобы получить те же результаты:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="d6a2a-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="d6a2a-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="d6a2a-519">Кроме того можно назначить атрибут ClientValidationEnabled в любой контроллер, чтобы иметь пользовательское поведение.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="d6a2a-520">Откройте **Create.cshtml** в **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="d6a2a-521">Убедитесь, что следующие файлы скрипта **jquery.validate** и **jquery.validate.unobtrusive**, на которые ссылается представление контекстной &quot; **~/bundles/jqueryval** &quot; пакета.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-522">Все эти библиотеки jQuery, включаются в новые проекты MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="d6a2a-523">Можно найти дополнительные библиотеки в **/скрипты** папки проекта вы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="d6a2a-524">Чтобы эта проверка работы библиотеки, необходимо добавить ссылку на библиотеку jQuery framework.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="d6a2a-525">Так как эта ссылка уже добавлена в  **\_Layout.cshtml** файл, не требуется добавить его в данном представлении.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="d6a2a-526">Задача 3. запуск приложения с помощью ненавязчивого jQuery проверки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="d6a2a-527">В этой задаче вы проверите, **StoreManager** создать представление шаблона выполняет проверки на стороне клиента с использованием библиотеки jQuery, при создании пользователем нового альбома.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="d6a2a-528">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="d6a2a-529">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-529">The project starts in the Home page.</span></span> <span data-ttu-id="d6a2a-530">Обзор **/StoreManager/Create** и нажмите кнопку **создать** без заполнения форму, чтобы убедиться, чтобы получить сообщения проверки:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="d6a2a-531">![Проверка клиента с помощью jQuery включен](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "проверка клиента с помощью jQuery включен")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="d6a2a-532">*Проверка клиента с помощью jQuery включен*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="d6a2a-533">В браузере откройте исходный код для создания представления:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="d6a2a-534">Для каждого правила клиентской проверки ненавязчивого jQuery добавляет атрибут с данными-val -*rulename*=&quot;*сообщение*&quot;.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="d6a2a-535">Ниже приведен список тегов, Unobtrusive jQuery вставляет в поле ввода html для выполнения проверки клиента:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="d6a2a-536">Данные val</span><span class="sxs-lookup"><span data-stu-id="d6a2a-536">Data-val</span></span>
    > - <span data-ttu-id="d6a2a-537">Val номер данных</span><span class="sxs-lookup"><span data-stu-id="d6a2a-537">Data-val-number</span></span>
    > - <span data-ttu-id="d6a2a-538">Диапазон данных — val</span><span class="sxs-lookup"><span data-stu-id="d6a2a-538">Data-val-range</span></span>
    > - <span data-ttu-id="d6a2a-539">Данные val диапазона min / данных val диапазона max.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-539">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="d6a2a-540">Требуются данные val</span><span class="sxs-lookup"><span data-stu-id="d6a2a-540">Data-val-required</span></span>
    > - <span data-ttu-id="d6a2a-541">Длина данных — val</span><span class="sxs-lookup"><span data-stu-id="d6a2a-541">Data-val-length</span></span>
    > - <span data-ttu-id="d6a2a-542">Данные val длина max / данных val длина min</span><span class="sxs-lookup"><span data-stu-id="d6a2a-542">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="d6a2a-543">Все значения заполняются модели **заметки к данным**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="d6a2a-544">Затем всю логику, которая работает на стороне сервера может выполняться на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="d6a2a-545">Например цена атрибут имеет следующие заметки к данным в модели:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-545">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="d6a2a-546">После использования ненавязчивого jQuery, — созданный код:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-546">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d6a2a-547">Сводка</span><span class="sxs-lookup"><span data-stu-id="d6a2a-547">Summary</span></span>

<span data-ttu-id="d6a2a-548">Выполнив это Практическое лабораторное занятие было изучено пользователи могли изменять данные, хранящиеся в базе данных с использованием следующих:</span><span class="sxs-lookup"><span data-stu-id="d6a2a-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="d6a2a-549">Действия контроллера, например индекс, создание, изменение, удаление</span><span class="sxs-lookup"><span data-stu-id="d6a2a-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="d6a2a-550">ASP.NET MVC функции формирования шаблонов для отображение свойств таблицы HTML</span><span class="sxs-lookup"><span data-stu-id="d6a2a-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="d6a2a-551">Возникают пользовательских вспомогательных методов HTML для улучшения пользователя</span><span class="sxs-lookup"><span data-stu-id="d6a2a-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="d6a2a-552">Методы действий, реагирующие на HTTP-GET или HTTP POST вызовов</span><span class="sxs-lookup"><span data-stu-id="d6a2a-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="d6a2a-553">Шаблон общего редактора для похожих шаблонов представления, такие как создание и изменение</span><span class="sxs-lookup"><span data-stu-id="d6a2a-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="d6a2a-554">Элементы формы, например раскрывающиеся списки</span><span class="sxs-lookup"><span data-stu-id="d6a2a-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="d6a2a-555">Заметок к данным для проверки модели</span><span class="sxs-lookup"><span data-stu-id="d6a2a-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="d6a2a-556">С помощью ненавязчивого библиотеки jQuery проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="d6a2a-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d6a2a-557">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="d6a2a-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d6a2a-558">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="d6a2a-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d6a2a-559">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d6a2a-560">Последовательно выберите пункты [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d6a2a-561">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; *Visual Studio Express 2012 для Web с пакетом Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="d6a2a-562">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-562">Click on **Install Now**.</span></span> <span data-ttu-id="d6a2a-563">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d6a2a-564">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d6a2a-565">![Установка Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d6a2a-566">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d6a2a-567">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="d6a2a-569">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d6a2a-570">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-570">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="d6a2a-572">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-572">*Installation progress*</span></span>
6. <span data-ttu-id="d6a2a-573">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-573">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="d6a2a-575">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-575">*Installation completed*</span></span>
7. <span data-ttu-id="d6a2a-576">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d6a2a-577">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="d6a2a-579">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="d6a2a-580">Приложение б. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="d6a2a-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="d6a2a-581">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d6a2a-582">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d6a2a-583">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d6a2a-584">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="d6a2a-585">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="d6a2a-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="d6a2a-586">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d6a2a-587">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d6a2a-588">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d6a2a-589">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="d6a2a-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d6a2a-590">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="d6a2a-591">![Начните вводить имя фрагмента](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="d6a2a-592">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="d6a2a-593">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="d6a2a-594">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="d6a2a-595">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="d6a2a-596">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="d6a2a-597">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="d6a2a-598">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="d6a2a-599">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="d6a2a-600">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="d6a2a-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="d6a2a-601">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="d6a2a-602">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="d6a2a-603">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="d6a2a-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="d6a2a-604">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="d6a2a-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
