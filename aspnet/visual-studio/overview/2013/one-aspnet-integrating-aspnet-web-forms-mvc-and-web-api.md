---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Практическое лабораторное занятие: ASP.NET: интеграция веб-форм ASP.NET, MVC и веб-API | Документация Майкрософт'
author: rick-anderson
description: ASP.NET — это платформа для разработки веб-сайтов, приложений и служб с помощью специализированных технологий, таких как MVC, веб-API и другие. С расширением ASP.NET h...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 276207a6e7d2388ce53778928665c35de9327318
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837309"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="d5c5a-104">Практическое лабораторное занятие: ASP.NET: интеграция веб-форм ASP.NET, MVC и веб-API</span><span class="sxs-lookup"><span data-stu-id="d5c5a-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="d5c5a-105">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d5c5a-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d5c5a-106">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="d5c5a-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="d5c5a-107">ASP.NET — это платформа для разработки веб-сайтов, приложений и служб с помощью специализированных технологий, таких как MVC, веб-API и другие.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="d5c5a-108">С расширением ASP.NET с момента его создания и выраженное должны эти технологии интегрированы, предпринимались последних в работе по направлению к **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="d5c5a-109">Visual Studio 2013 представлены новой системы проектов, который позволит вам создавать приложения и использовать все технологии ASP.NET в одном проекте.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="d5c5a-110">Эта функция устраняет необходимость для выбора одной технологии в начале проекта и палочка с ним и вместо рекомендуют использовать несколько платформ ASP.NET, в рамках одного проекта.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="d5c5a-111">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="d5c5a-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="d5c5a-112">Обзор</span><span class="sxs-lookup"><span data-stu-id="d5c5a-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d5c5a-113">Цели</span><span class="sxs-lookup"><span data-stu-id="d5c5a-113">Objectives</span></span>

<span data-ttu-id="d5c5a-114">В этой практической работу вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="d5c5a-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d5c5a-115">Создание веб-сайта, на основе **One ASP.NET** тип проекта</span><span class="sxs-lookup"><span data-stu-id="d5c5a-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="d5c5a-116">Использование разных **ASP.NET** платформы, такие как **MVC** и **веб-API** в одном проекте</span><span class="sxs-lookup"><span data-stu-id="d5c5a-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="d5c5a-117">Определить основные компоненты **ASP.NET** приложения</span><span class="sxs-lookup"><span data-stu-id="d5c5a-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="d5c5a-118">Воспользоваться преимуществами **формирование шаблонов ASP.NET** framework автоматически создавать контроллеры и представления для выполнения операций CRUD на основе вашей модели классов</span><span class="sxs-lookup"><span data-stu-id="d5c5a-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="d5c5a-119">Предоставлять тот же набор сведений в форматах машины - и удобное для восприятия, с помощью подходящего средства для каждого задания</span><span class="sxs-lookup"><span data-stu-id="d5c5a-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d5c5a-120">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d5c5a-120">Prerequisites</span></span>

<span data-ttu-id="d5c5a-121">Для завершения этой практической работу требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="d5c5a-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="d5c5a-122">[Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="d5c5a-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="d5c5a-123">Visual Studio 2013 с обновлением 1</span><span class="sxs-lookup"><span data-stu-id="d5c5a-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d5c5a-124">Установка</span><span class="sxs-lookup"><span data-stu-id="d5c5a-124">Setup</span></span>

<span data-ttu-id="d5c5a-125">Для выполнения упражнений в этой практической работу, необходимо сначала настроить среду.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="d5c5a-126">Откройте Windows Explorer и перейдите к лаборатории **источника** папки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="d5c5a-127">Щелкните правой кнопкой мыши **Setup.cmd** и выберите **Запуск от имени администратора** для запуска процесса установки, будет настроить среду и установить фрагменты кода Visual Studio для этой лабораторной работы.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="d5c5a-128">Если отображается диалоговое окно контроля учетных записей, подтвердите действие для продолжения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="d5c5a-129">Убедитесь, что все зависимости для этой лабораторной работы вы вернули перед запуском программы установки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="d5c5a-130">Фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="d5c5a-130">Using the Code Snippets</span></span>

<span data-ttu-id="d5c5a-131">В этом документе лаборатории вам будет рекомендовано вставлять блоки кода.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d5c5a-132">Для удобства основная часть кода предоставляется как фрагменты кода Visual Studio, к которому можно получить из в Visual Studio 2013, чтобы избежать необходимости добавлять ее вручную.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="d5c5a-133">Каждого упражнения сопровождается начальный решения, расположенный в **начать** папку упражнения, чему вы сможете каждого упражнения, независимо от других.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="d5c5a-134">Имейте в виду, что фрагменты кода, которые добавляются во время атаки, отсутствуют эти стартовые решения и могут не работать, пока не будут выполнены упражнения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="d5c5a-135">В исходном коде для упражнения, вы также найдете **окончания** папку, содержащую решение Visual Studio с кодом, полученный в результате выполнения действий, описанных в соответствующий упражнении.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="d5c5a-136">Эти решения можно использовать в качестве руководства, если вам нужна дополнительная помощь, отвечая на это Практическое занятие.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d5c5a-137">Упражнения</span><span class="sxs-lookup"><span data-stu-id="d5c5a-137">Exercises</span></span>

<span data-ttu-id="d5c5a-138">Это Практическое занятие включает в себя следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="d5c5a-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d5c5a-139">Создание нового проекта Web Forms</span><span class="sxs-lookup"><span data-stu-id="d5c5a-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="d5c5a-140">Создание с помощью формирования шаблонов контроллера MVC</span><span class="sxs-lookup"><span data-stu-id="d5c5a-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="d5c5a-141">Создание контроллера Web API, с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="d5c5a-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="d5c5a-142">Предполагаемое время для выполнения этого лабораторного: **60 минут**</span><span class="sxs-lookup"><span data-stu-id="d5c5a-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="d5c5a-143">При первом запуске Visual Studio, необходимо выбрать один из предварительно определенных коллекций параметров.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="d5c5a-144">Каждой коллекции предопределенных соответствует конкретному стилю разработки и определяет макетов окон, поведение редактора, фрагменты кода IntelliSense и параметры диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="d5c5a-145">В этом лабораторном занятии описаны действия, необходимые для выполнения данной задачи в Visual Studio при использовании **обычные параметры разработки** коллекции.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="d5c5a-146">При выборе другого набора параметров для среды разработки, возможно, различия в действиях, которые следует учитывать.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="d5c5a-147">Упражнение 1: Создание нового проекта Web Forms</span><span class="sxs-lookup"><span data-stu-id="d5c5a-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="d5c5a-148">В этом упражнении вы создадите новый узел веб-форм в Visual Studio 2013 с использованием **One ASP.NET** единой опыт работы с проектами, которые позволят вам легко интегрировать компоненты веб-форм, MVC и веб-API в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="d5c5a-149">Затем изучите сгенерированное решение и определить его частей, и наконец вы увидите веб-сайта в действии.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="d5c5a-150">Задача 1 – Создание нового сайта с помощью одного интерфейса ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5c5a-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="d5c5a-151">В этой задаче вы начнете создание нового веб-сайта в Visual Studio на основе **One ASP.NET** тип проекта.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="d5c5a-152">**One ASP.NET** объединяет все технологии ASP.NET и предоставляет вам возможность комбинировать и сопоставлять их при необходимости.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="d5c5a-153">Затем будет распознавать различных компонентов веб-форм, MVC и веб-API, находящиеся рядом друг с другом в вашем приложении.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="d5c5a-154">Откройте **Visual Studio Express 2013 для Web** и выберите **файл | Создать проект...**  для запуска нового решения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Создание нового проекта](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="d5c5a-156">*Создание нового проекта*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="d5c5a-157">В **новый проект** выберите **веб-приложение ASP.NET** под **Visual C# | Web** вкладку и убедитесь, что **.NET Framework 4.5** выбран.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="d5c5a-158">Назовите проект *MyHybridSite*, выберите **расположение** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Новый проект веб-приложения ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="d5c5a-160">*Создание нового проекта веб-приложения ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="d5c5a-161">В **новый проект ASP.NET** выберите **веб-форм** шаблона и выберите **MVC** и **веб-API** параметры.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="d5c5a-162">Кроме того, убедитесь, что **проверки подлинности** параметру присваивается **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="d5c5a-163">Нажмите кнопку **ОК** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-163">Click **OK** to continue.</span></span>

    ![Создание нового проекта с помощью шаблона веб-форм, включая компоненты веб-API и MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="d5c5a-165">*Создание нового проекта с помощью шаблона веб-форм, включая компоненты веб-API и MVC*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="d5c5a-166">Теперь вы можете изучить структуру сгенерированное решение.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-166">You can now explore the structure of the generated solution.</span></span>

    ![Изучение созданного решения](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="d5c5a-168">*Изучение созданного решения*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="d5c5a-169">**Учетная запись:** эта папка содержит страницы веб-формы для регистрации, вход и управление учетными записями пользователей приложения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="d5c5a-170">Эта папка будет добавлена при **учетные записи отдельных пользователей** выбран параметр проверки подлинности во время настройки шаблона проекта веб-форм.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="d5c5a-171">**Модели:** эта папка будет содержать классы, представляющие данные приложения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="d5c5a-172">**Контроллеры** и **представления**: этих папок являются обязательными для **ASP.NET MVC** и **веб-API ASP.NET** компонентов.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="d5c5a-173">Рассмотрим в следующих упражнениях технологии MVC и веб-API.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="d5c5a-174">**Default.aspx**, **Contact.aspx** и **About.aspx** файлы, предварительно определенные страницы веб-форм, которые можно использовать в качестве отправных точек для создания страниц, относящиеся к вашей приложение.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="d5c5a-175">Программная логика этих файлов находится в отдельном файле, который называется &quot;кода&quot; файл, который имеет &quot;. aspx.vb&quot; или &quot;. aspx.cs&quot; расширения (в зависимости от язык, используемый).</span><span class="sxs-lookup"><span data-stu-id="d5c5a-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="d5c5a-176">Логику кода выполняется на сервере и динамически создает выходные данные HTML для страницы.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="d5c5a-177">**Site.Master** и **Site.Mobile.Master** страницы определяют внешний вид и стандартное поведение всех страниц в приложении.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="d5c5a-178">Дважды щелкните **Default.aspx** файл для просмотра содержимого страницы.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Просмотр страницы Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="d5c5a-180">*Просмотр страницы Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-181">**Страницы** директивы в верхней части файла определяет атрибуты страницы веб-форм.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="d5c5a-182">К примеру **MasterPageFile** атрибут указывает путь к главной странице — в этом случае *Site.Master* страницы - и **Inherits** атрибут определяет Класс фонового кода для страницы, чтобы наследовать.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="d5c5a-183">Этот класс находится в файле определяется **фонового кода** атрибута.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="d5c5a-184">**Asp: Content** управления содержит фактическое содержимое страницы (текст, разметку и элементы управления) и сопоставляется с **asp: ContentPlaceHolder** управления на главной странице.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="d5c5a-185">В этом случае будет отображено содержимое страницы внутри *MainContent* управления, определенных в *Site.Master* страницы.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="d5c5a-186">Разверните **приложения\_запустить** папку и обратите внимание, что **WebApiConfig.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="d5c5a-187">Visual Studio включены этот файл в сгенерированное решение, так как вы включили веб-API, при настройке проекта с помощью шаблона One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="d5c5a-188">Откройте **WebApiConfig.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="d5c5a-189">В *WebApiConfig* направляет класса, вы найдете конфигурации, связанные с веб-API, которая сопоставляет HTTP **контроллеров веб-API**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="d5c5a-190">Откройте **RouteConfig.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="d5c5a-191">Внутри *RegisterRoutes* вы найдете конфигурации, связанной с моделью MVC, которая сопоставляет маршруты HTTP, чтобы метод **контроллеров MVC**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="d5c5a-192">Задача 2 – при запуске решения</span><span class="sxs-lookup"><span data-stu-id="d5c5a-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="d5c5a-193">В этой задаче будет запустите сгенерированное решение и изучение приложения и некоторые его функции, такие как переписывание URL-адресов и встроенная проверка подлинности.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="d5c5a-194">Чтобы запустить решение, нажмите клавишу **F5** или нажмите кнопку **запустить** расположенную на панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="d5c5a-195">На домашней странице приложения откроется в браузере.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-195">The application home page should open in the browser.</span></span>

    ![При запуске решения](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="d5c5a-197">Убедитесь, что вызываются на страницах веб-форм.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="d5c5a-198">Чтобы сделать это, добавьте **/contact.aspx** на URL-адрес в адресной строке и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Понятные URL-адреса](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="d5c5a-200">*Понятные URL-адреса*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-201">Как вы видите, URL-адрес примет **/обратитесь в службу**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="d5c5a-202">Начиная с **ASP.NET 4**, были добавлены возможности маршрутизации URL-адрес для веб-форм, поэтому вы можете писать URL-адреса, например *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* вместо  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="d5c5a-203">Дополнительные сведения см. [маршрутизации URL-адресов](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="d5c5a-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="d5c5a-204">Теперь вы изучить процесс проверки подлинности, интегрировать в приложение.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="d5c5a-205">Чтобы сделать это, нажмите кнопку **зарегистрировать** в правом верхнем углу страницы.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Регистрация нового пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="d5c5a-207">*Регистрация нового пользователя*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-207">*Registering a new user*</span></span>
4. <span data-ttu-id="d5c5a-208">В **зарегистрировать** введите **имя пользователя** и **пароль**, а затем нажмите кнопку **зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Страница «Регистрация»](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="d5c5a-210">*Страница «Регистрация»*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-210">*Register page*</span></span>
5. <span data-ttu-id="d5c5a-211">Приложение регистрирует новую учетную запись, и пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Пользователь прошел проверку подлинности](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="d5c5a-213">*Пользователь прошел проверку подлинности*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-213">*User authenticated*</span></span>
6. <span data-ttu-id="d5c5a-214">Вернитесь к Visual Studio и нажать клавишу **SHIFT + F5** остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="d5c5a-215">Упражнение 2: Создание с помощью формирования шаблонов контроллера MVC</span><span class="sxs-lookup"><span data-stu-id="d5c5a-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="d5c5a-216">В этом упражнении будет воспользоваться преимуществами платформы формирование шаблонов ASP.NET в Visual Studio для создания контроллера ASP.NET MVC 5 с действиями и представлениями Razor для выполнения операций CRUD, не написав ни строчки кода.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="d5c5a-217">Процесс формирования шаблонов использует Entity Framework Code First для создания контекста данных и схемы базы данных в базе данных SQL.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="d5c5a-218">**О платформе Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="d5c5a-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="d5c5a-219">Entity Framework (EF) — это объектно реляционного сопоставления (ORM), которая позволяет создавать приложения для доступа к данным, работающие с концептуальной моделью приложения вместо программирования, напрямую с помощью реляционной схемой хранения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="d5c5a-220">Entity Framework Code First рабочий процесс моделирования можно использовать собственные классы домена для представления модели, который использует EF при выполнении запроса, функции отслеживания изменений и обновления.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="d5c5a-221">С помощью Code First рабочий процесс разработки, вы не обязательно должны начинаться приложения путем создания базы данных или указание схемы.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="d5c5a-222">Вместо этого можно написать стандартных классов .NET, которые определяют наиболее подходящий объектов модели предметной области для вашего приложения, и платформа Entity Framework автоматически создаст базу данных.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="d5c5a-223">Дополнительные сведения о платформе Entity Framework [здесь](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="d5c5a-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="d5c5a-224">Задача 1 – Создание новой модели</span><span class="sxs-lookup"><span data-stu-id="d5c5a-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="d5c5a-225">Теперь мы определим **Person** класс, который будет модели, использованный во время формирования шаблонов для создания контроллера MVC и представлений.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="d5c5a-226">Сначала нужно создать **Person** класс модели и операции CRUD в контроллере будут автоматически созданы с помощью функции формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="d5c5a-227">Откройте **Visual Studio Express 2013 для Web** и **MyHybridSite.sln** решение находится в **источника/Ex2-MvcScaffolding/начало** папки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="d5c5a-228">Кроме того можно продолжить с решением, полученный в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="d5c5a-229">В **обозревателе решений**, щелкните правой кнопкой мыши **моделей** папке **MyHybridSite** проекта и выберите **Add | Класс...** .</span><span class="sxs-lookup"><span data-stu-id="d5c5a-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Добавление класса модели Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="d5c5a-231">*Добавление класса модели Person*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="d5c5a-232">В **Добавление нового элемента** диалоговом окне имя файла *Person.cs* и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Создание класса модели Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="d5c5a-234">*Создание класса модели Person*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="d5c5a-235">Замените содержимое **Person.cs** файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="d5c5a-236">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="d5c5a-237">(Code Snippet - *PersonClass BringingTogetherOneAspNet - Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="d5c5a-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="d5c5a-238">В **обозревателе решений**, щелкните правой кнопкой мыши **MyHybridSite** проекта и выберите **построения**, или нажмите клавишу **CTRL + SHIFT + B** для сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="d5c5a-239">Задача 2 – Создание контроллера MVC</span><span class="sxs-lookup"><span data-stu-id="d5c5a-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="d5c5a-240">Теперь, когда **Person** создания модели, вы будете использовать формирование шаблонов ASP.NET MVC с Entity Framework для создания действия CRUD контроллера и представлений для **Person**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="d5c5a-241">В **обозревателе решений**, щелкните правой кнопкой мыши **контроллеров** папке **MyHybridSite** проекта и выберите **Add | Создать шаблонный элемент...** .</span><span class="sxs-lookup"><span data-stu-id="d5c5a-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Создание нового формирования шаблонов контроллера](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="d5c5a-243">*Создание нового контроллера Скаффолдинга*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="d5c5a-244">В **Добавление шаблона** выберите **контроллер MVC 5 с представлениями, использующий Entity Framework** и нажмите кнопку **Add.**</span><span class="sxs-lookup"><span data-stu-id="d5c5a-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Выбрав контроллер MVC 5 с представлениями и Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="d5c5a-246">*Выбрав контроллер MVC 5 с представлениями и Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="d5c5a-247">Задайте *MvcPersonController* как **имя контроллера**выберите **использовать асинхронные действия контроллера** и выберите **Person (MyHybridSite.Models)**  как **класс модели**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Добавление контроллера MVC с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="d5c5a-249">*Добавление контроллера MVC с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="d5c5a-250">В разделе **класс контекста данных**, нажмите кнопку **новый контекст данных...** .</span><span class="sxs-lookup"><span data-stu-id="d5c5a-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Создать новый контекст данных](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="d5c5a-252">*Создать новый контекст данных*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="d5c5a-253">В **новый контекст данных** диалоговом окне имя новый контекст данных *PersonContext* и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Создание нового PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="d5c5a-255">*Создание нового типа PersonContext*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="d5c5a-256">Нажмите кнопку **добавить** для создания нового контроллера для **Person** с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="d5c5a-257">Visual Studio создаст действий контроллера, контекст данных Person и представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![После создания контроллер MVC с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="d5c5a-259">*После создания контроллер MVC с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="d5c5a-260">Откройте **MvcPersonController.cs** файл **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="d5c5a-261">Обратите внимание на то, что методы действия CRUD были созданы автоматически.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="d5c5a-262">Выбрав **использовать асинхронные действия контроллера** смарт-теге формирование шаблонов параметров в предыдущих шагах, Visual Studio создает асинхронные методы действия для всех действий, которые включают в себя доступ к контексту данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="d5c5a-263">Рекомендуется использовать асинхронные методы действия для выполняющейся длительное время, не связаны с ЦП запросы для предотвращения блокировки веб-сервера от выполнения операций во время обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="d5c5a-264">Задача 3 – при запуске решения</span><span class="sxs-lookup"><span data-stu-id="d5c5a-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="d5c5a-265">В этой задаче будет выполняться представлений для решения еще раз, чтобы убедиться, что **Person** работают надлежащим образом.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="d5c5a-266">Вы добавите новый пользователь, чтобы проверить, успешно сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="d5c5a-267">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="d5c5a-268">Перейдите к **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="d5c5a-269">Должна появиться шаблонный представление, отображающее список пользователей.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="d5c5a-270">Нажмите кнопку **Create New** для добавления нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-270">Click **Create New** to add a new person.</span></span>

    ![Переход к сформированные представления MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="d5c5a-272">*Переход к сформированные представления MVC*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="d5c5a-273">В **создать** просмотреть, укажите **имя** и **возраст** человека, и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Добавление нового пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="d5c5a-275">*Добавление нового пользователя*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-275">*Adding a new person*</span></span>
5. <span data-ttu-id="d5c5a-276">Новый пользователь добавляется в список.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-276">The new person is added to the list.</span></span> <span data-ttu-id="d5c5a-277">В списке элементов выберите **сведения** для отображения представления сведений пользователя.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="d5c5a-278">Затем в **сведения** , щелкните **обратно к списку** чтобы вернуться к представлению списка.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Представление сведений пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="d5c5a-280">*Представление сведений пользователя*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-280">*Person's details view*</span></span>
6. <span data-ttu-id="d5c5a-281">Нажмите кнопку **удалить** ссылка для удаления пользователя.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="d5c5a-282">В **удалить** , щелкните **удалить** для подтверждения операции.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Удаление пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="d5c5a-284">*Удаление пользователя*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-284">*Deleting a person*</span></span>
7. <span data-ttu-id="d5c5a-285">Вернитесь к Visual Studio и нажать клавишу **SHIFT + F5** остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="d5c5a-286">Упражнение 3: Создание контроллер веб-API, с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="d5c5a-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="d5c5a-287">Платформа веб-API является частью стека ASP.NET и предназначенных для упрощения реализации службы HTTP, как правило, отправки и получения данных в формате JSON или XML через API-интерфейсов RESTful.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="d5c5a-288">В этом упражнении будет использовать формирование шаблонов ASP.NET снова создать контроллер Web API.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="d5c5a-289">Применяются те же **Person** и **PersonContext** классов из предыдущего упражнения для предоставляют одинаковые данные пользователя в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="d5c5a-290">Вы увидите, как можно предоставить те же ресурсы по-разному внутри одного приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="d5c5a-291">Задача 1 – Создание контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="d5c5a-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="d5c5a-292">В этой задаче вы создадите новый **контроллер Web API** , будет представлять данные пользователя в машинные команды компьютера в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="d5c5a-293">Если еще не открыт, откройте **Visual Studio Express 2013 для Web** и откройте **MyHybridSite.sln** решение находится в **источника/Ex3-веб-API/начало** папки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="d5c5a-294">Кроме того можно продолжить с решением, полученный в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-295">При запуске с помощью решения Begin из Упражнение 3, нажмите клавишу **CTRL + SHIFT + B** для сборки решения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="d5c5a-296">В **обозревателе решений**, щелкните правой кнопкой мыши **контроллеров** папке **MyHybridSite** проекта и выберите **Add | Создать шаблонный элемент...** .</span><span class="sxs-lookup"><span data-stu-id="d5c5a-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Создание нового формирования шаблонов контроллера](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="d5c5a-298">*Создание нового формирования шаблонов контроллера*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="d5c5a-299">В **Добавление шаблона** выберите **веб-API** в области слева, затем **контроллер Web API 2 с действиями, использующий Entity Framework** в средней области и нажмите кнопку  **Добавите.**</span><span class="sxs-lookup"><span data-stu-id="d5c5a-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="d5c5a-300">![Выбрав контроллер Web API 2 с действиями и Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "выбрав контроллер Web API 2 с действиями и Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="d5c5a-301">*Выбрав контроллер Web API 2 с действиями и Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="d5c5a-302">Задайте *ApiPersonController* как **имя контроллера**выберите **использовать асинхронные действия контроллера** и выберите **Person (MyHybridSite.Models)**  и **PersonContext (MyHybridSite.Models)** как **модели** и **контекст данных** классы соответственно.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="d5c5a-303">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-303">Then click **Add**.</span></span>

    <span data-ttu-id="d5c5a-304">![Добавление контроллер веб-API с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Добавление контроллера в веб-API с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="d5c5a-305">*Добавление контроллера в веб-API с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="d5c5a-306">Visual Studio создаст **ApiPersonController** класс с четырьмя действиями CRUD для работы с данными.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="d5c5a-307">![После создания контроллера веб-API с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "после создания контроллера веб-API с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="d5c5a-308">*После создания контроллера веб-API с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="d5c5a-309">Откройте **ApiPersonController.cs** файл и проверьте *GetPeople* метода действия.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="d5c5a-310">Этот метод отправляет запрос в поле db **PersonContext** типа для получения данных пользователей.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="d5c5a-311">Теперь обратите внимание, что с комментарием над определение метода.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="d5c5a-312">Он предоставляет URI, который представляет это действие, который будет использоваться в следующей задаче.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="d5c5a-313">По умолчанию веб-API настроен для перехвата запросов к */API* путь, чтобы избежать конфликтов с помощью контроллеров MVC.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="d5c5a-314">Если вам нужно изменить этот параметр, см. раздел [маршрутизации в ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d5c5a-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="d5c5a-315">Задача 2 – при запуске решения</span><span class="sxs-lookup"><span data-stu-id="d5c5a-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="d5c5a-316">В этой задаче будет использоваться в Internet Explorer **средства разработчика F12** проверяемый полный ответ от контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="d5c5a-317">Вы увидите, как можно записывать сетевой трафик, чтобы получить дополнительные сведения о данных приложения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="d5c5a-318">Убедитесь, что **Internet Explorer** выбран в **запустить** расположенную на панели инструментов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Параметр Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="d5c5a-320">**Средства разработчика F12** имеют широкий набор функциональных возможностей, не охваченных этого практического занятия.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="d5c5a-321">Если вы хотите узнать больше об этом, см. раздел [с помощью средств разработчика F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="d5c5a-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="d5c5a-322">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-323">Чтобы правильно выполнить эту задачу, приложение должно иметь данные.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="d5c5a-324">Если пустой базы данных, можно вернуться к задаче 3 в упражнении 2 и выполните действия по созданию нового пользователя, с помощью представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="d5c5a-325">В браузере, нажмите клавишу **F12** открыть **средств разработчика** панели.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="d5c5a-326">Нажмите клавишу **CTRL** + **4** или нажмите кнопку **сети** значок, а затем щелкните кнопку с зеленой стрелкой, чтобы начать отслеживать сетевой трафик.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="d5c5a-327">![Запуск записи сетевых данных веб-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "записи сетевых данных инициирует веб-API")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="d5c5a-328">*Запуск записи сетевых данных веб-API*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="d5c5a-329">Добавление **api/ApiPerson** на URL-адрес в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="d5c5a-330">Теперь проверьте сведения об ответе от **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="d5c5a-331">![Извлечение данных пользователя через веб-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "извлечение данных пользователя через веб-API")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="d5c5a-332">*Извлечение данных пользователя через веб-API*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-333">После завершения загрузки, вам будет предложено задать действие с помощью загруженного файла.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="d5c5a-334">Не закрывайте диалоговое окно, чтобы иметь возможность смотреть содержимое ответа через окно инструментов для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="d5c5a-335">Теперь будет проверять текст ответа.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="d5c5a-336">Чтобы сделать это, нажмите кнопку **сведения** вкладке и нажмите кнопку **текст ответа**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="d5c5a-337">Вы можете проверить, что загруженные данные — это список объектов со свойствами **идентификатор**, **имя** и **возраст** , которые соответствуют **Person** класс.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="d5c5a-338">![Просмотр веб-текст ответа API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "просмотра веб-текст ответа API")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="d5c5a-339">*Текст ответа API Web просмотра*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="d5c5a-340">Задача 3 – добавление справки API веб-страниц</span><span class="sxs-lookup"><span data-stu-id="d5c5a-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="d5c5a-341">При создании веб-API, полезно создать страницу справки, чтобы другие разработчики будете знать, как вызвать API.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="d5c5a-342">Можно создать и обновлять страницы документации вручную, но лучше автоматически создавать их, чтобы не пришлось выполнять работу обслуживания.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="d5c5a-343">В этой задаче будет использовать пакет Nuget для автоматического создания страниц справки веб-API в решение.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="d5c5a-344">Из **средства** меню в Visual Studio, выберите пункт **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="d5c5a-345">В **консоль диспетчера пакетов** окно, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d5c5a-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="d5c5a-346">**Microsoft.AspNet.WebApi.HelpPage** пакет устанавливает необходимые сборки и добавляет представлений MVC для страницы справки, в разделе **области/HelpPage** папки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="d5c5a-347">![Область HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage область")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="d5c5a-348">*Область HelpPage*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="d5c5a-349">По умолчанию с помощью страницы имеют заполнитель строки для документации.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="d5c5a-350">Комментарии XML-документации можно использовать для создания документации.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="d5c5a-351">Чтобы включить эту функцию, откройте **HelpPageConfig.cs** файл, расположенный в **областей, приложения или HelpPage\_запустить** папки и раскомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="d5c5a-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="d5c5a-352">В **обозревателе решений**, щелкните правой кнопкой мыши проект **MyHybridSite**выберите **свойства** и нажмите кнопку **построения** вкладки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="d5c5a-353">![Вкладка "сборка"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Создание раздела")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="d5c5a-354">*Вкладка "сборка"*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-354">*Build tab*</span></span>
5. <span data-ttu-id="d5c5a-355">В разделе **вывода**выберите **XML-файл документации**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="d5c5a-356">В поле ввода введите **приложения\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="d5c5a-357">![Выходные данные раздела на вкладке "Построение"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "вывода раздела на вкладке \"Построение\"")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="d5c5a-358">*Раздел выходных данных на вкладке "Построение"*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="d5c5a-359">Нажмите клавишу **CTRL** + **S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="d5c5a-360">Откройте **ApiPersonController.cs** файла из **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="d5c5a-361">Введите новую строку между *GetPeople* сигнатуру метода и */ / GET api/ApiPerson* комментарий, а затем введите три косые черты.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-362">Visual Studio автоматически добавляет XML-элементы, которые определяют документацию по методу.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="d5c5a-363">Добавьте текст сводки и возвращаемое значение для *GetPeople* метод.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="d5c5a-364">Он должен выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="d5c5a-365">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="d5c5a-366">Добавление **/help** URL-адрес в адресной строке, чтобы перейти на страницу справки.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="d5c5a-367">![Страница справки веб-API ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "страница справки веб-API ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="d5c5a-368">*Страница справки веб-API ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d5c5a-369">Основное содержимое страницы — это таблица, API-интерфейсов, сгруппированные по контроллера.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="d5c5a-370">Записи таблицы создаются динамически, с помощью **IApiExplorer** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="d5c5a-371">Если добавить или обновить контроллер API таблицы будут обновляться автоматически в следующий раз вы создадите приложение.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="d5c5a-372">**API** столбце перечислены метод HTTP и относительного URI.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="d5c5a-373">**Описание** столбец содержит сведения, которые были извлечены из документации по методу.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="d5c5a-374">Обратите внимание, что описание, добавляемого перед определением метода отображается в столбце «Описание».</span><span class="sxs-lookup"><span data-stu-id="d5c5a-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="d5c5a-375">![Описание метода API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "описание метода API")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="d5c5a-376">*Описание метода API*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-376">*API method description*</span></span>
13. <span data-ttu-id="d5c5a-377">Выберите один из методов API, чтобы перейти на страницу с более подробными сведениями, включая примеры текстов ответа.</span><span class="sxs-lookup"><span data-stu-id="d5c5a-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="d5c5a-378">![Странице подробной информации](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "страницу подробных сведений")</span><span class="sxs-lookup"><span data-stu-id="d5c5a-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="d5c5a-379">*Подробные сведения страницы*</span><span class="sxs-lookup"><span data-stu-id="d5c5a-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d5c5a-380">Сводка</span><span class="sxs-lookup"><span data-stu-id="d5c5a-380">Summary</span></span>

<span data-ttu-id="d5c5a-381">Выполнив этой практической работу вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="d5c5a-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="d5c5a-382">Создание нового веб-приложения, используя один интерфейс ASP.NET в Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d5c5a-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="d5c5a-383">Интегрировать несколько технологий ASP.NET в один единый проект</span><span class="sxs-lookup"><span data-stu-id="d5c5a-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="d5c5a-384">Создание контроллеров и представлений MVC из классах модели с помощью формирования шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5c5a-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="d5c5a-385">Создание контроллеров веб-API, которые используют функции, такие как асинхронного программирования и доступа к данным через Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d5c5a-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="d5c5a-386">Автоматически создавать веб-страницы справки API контроллеров</span><span class="sxs-lookup"><span data-stu-id="d5c5a-386">Automatically generate Web API Help Pages for your controllers</span></span>
