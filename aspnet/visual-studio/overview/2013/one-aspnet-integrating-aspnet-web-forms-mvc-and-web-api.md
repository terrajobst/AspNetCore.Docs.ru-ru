---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Практическое лабораторное занятие: Один ASP.NET: интеграция веб-форм ASP.NET, MVC и веб-API | Документы Microsoft"
author: rick-anderson
description: "ASP.NET — это платформа для создания веб-сайтов, приложений и служб с помощью специальных технологий, таких как MVC, веб-API и другие. С расширением ASP.NET h..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="62232-104">Практическое лабораторное занятие: Один ASP.NET: интеграция веб-форм ASP.NET, MVC и веб-API</span><span class="sxs-lookup"><span data-stu-id="62232-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="62232-105">по [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="62232-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="62232-106">Загрузите комплект учебных материалов лагеря Web</span><span class="sxs-lookup"><span data-stu-id="62232-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="62232-107">ASP.NET — это платформа для создания веб-сайтов, приложений и служб с помощью специальных технологий, таких как MVC, веб-API и другие.</span><span class="sxs-lookup"><span data-stu-id="62232-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="62232-108">С расширением ASP.NET с момента его создания и явных должны иметь эти технологии интеграции, предпринимались последних в работе по направлению к **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="62232-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="62232-109">Visual Studio 2013 вводит новую систему единой проектов, в которой можно построить приложение и использовать все технологии ASP.NET в одном проекте.</span><span class="sxs-lookup"><span data-stu-id="62232-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="62232-110">Эта функция избавляет от необходимости выбора одной технологии в начале проекта и манипулятор с ним и вместо этого рекомендует использование нескольких платформ ASP.NET в рамках одного проекта.</span><span class="sxs-lookup"><span data-stu-id="62232-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="62232-111">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="62232-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="62232-112">Обзор</span><span class="sxs-lookup"><span data-stu-id="62232-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="62232-113">Цели</span><span class="sxs-lookup"><span data-stu-id="62232-113">Objectives</span></span>

<span data-ttu-id="62232-114">В этой практической вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="62232-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="62232-115">Создать веб-сайт, на основе **One ASP.NET** тип проекта</span><span class="sxs-lookup"><span data-stu-id="62232-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="62232-116">Использование разных **ASP.NET** платформ, например **MVC** и **веб-API** в одном проекте</span><span class="sxs-lookup"><span data-stu-id="62232-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="62232-117">Определить основные компоненты **ASP.NET** приложения</span><span class="sxs-lookup"><span data-stu-id="62232-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="62232-118">Воспользоваться преимуществами **формирование шаблонов ASP.NET** framework автоматически создавать контроллеры и представления для выполнения операций CRUD, основанные на классах вашей модели</span><span class="sxs-lookup"><span data-stu-id="62232-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="62232-119">Предоставлять тот же набор сведений в машины - и восприятия форматы, используя правильное средство для каждого задания</span><span class="sxs-lookup"><span data-stu-id="62232-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="62232-120">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="62232-120">Prerequisites</span></span>

<span data-ttu-id="62232-121">Для завершения этой практической требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="62232-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="62232-122">[Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/) или выше</span><span class="sxs-lookup"><span data-stu-id="62232-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="62232-123">Visual Studio 2013 с обновлением 1</span><span class="sxs-lookup"><span data-stu-id="62232-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="62232-124">Установка</span><span class="sxs-lookup"><span data-stu-id="62232-124">Setup</span></span>

<span data-ttu-id="62232-125">Чтобы выполнить упражнения в этой практической, необходимо настроить среду.</span><span class="sxs-lookup"><span data-stu-id="62232-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="62232-126">Откройте проводник и найдите в лаборатории **источника** папки.</span><span class="sxs-lookup"><span data-stu-id="62232-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="62232-127">Щелкните правой кнопкой мыши **Setup.cmd** и выберите **Запуск от имени администратора** для запуска процесса установки, будет настроить среду и установить фрагменты кода Visual Studio для этой лабораторной работы.</span><span class="sxs-lookup"><span data-stu-id="62232-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="62232-128">Если отображается диалоговое окно контроля учетных записей, подтвердите действие для продолжения.</span><span class="sxs-lookup"><span data-stu-id="62232-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="62232-129">Убедитесь, что все зависимости для этой лабораторной работы вы вернули перед запуском программы установки.</span><span class="sxs-lookup"><span data-stu-id="62232-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="62232-130">Фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="62232-130">Using the Code Snippets</span></span>

<span data-ttu-id="62232-131">В документе лаборатории будет предложено вставлять блоки кода.</span><span class="sxs-lookup"><span data-stu-id="62232-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="62232-132">Visual Studio к фрагментам кода, к которому можно получить из в Visual Studio 2013, чтобы избежать необходимости вручную добавить большая часть кода предоставляется для удобства.</span><span class="sxs-lookup"><span data-stu-id="62232-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="62232-133">Каждого упражнения сопровождается начальный решений, расположенный в **начать** папку расчетов, позволяющий выполнить каждого упражнения независимо от других.</span><span class="sxs-lookup"><span data-stu-id="62232-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="62232-134">Имейте в виду, что фрагменты кода, которые добавляются во время упражнения, отсутствуют на их запуск решения и могут не работать до завершения этого упражнения.</span><span class="sxs-lookup"><span data-stu-id="62232-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="62232-135">В исходном коде для упражнения, вы также найдете **окончания** папку, содержащую решение Visual Studio с кодом, полученный в результате выполнения действий в соответствующий упражнении.</span><span class="sxs-lookup"><span data-stu-id="62232-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="62232-136">Эти решения можно использовать как рекомендации, если вам нужна дополнительная помощь, отвечая на этой практической.</span><span class="sxs-lookup"><span data-stu-id="62232-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="62232-137">Упражнения</span><span class="sxs-lookup"><span data-stu-id="62232-137">Exercises</span></span>

<span data-ttu-id="62232-138">Данная практическая работа включает следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="62232-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="62232-139">Создание нового проекта Web Forms</span><span class="sxs-lookup"><span data-stu-id="62232-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="62232-140">Создание контроллера MVC с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="62232-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="62232-141">Создание контроллера Web API, с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="62232-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="62232-142">Предполагаемое время для выполнения этого занятия: **60 минут**</span><span class="sxs-lookup"><span data-stu-id="62232-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="62232-143">При первом запуске Visual Studio, необходимо выбрать одну из коллекций предварительно определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="62232-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="62232-144">Каждая предопределенная коллекция соответствует конкретному стилю разработки и определяет макетов окон, поведение редактора, фрагменты кода IntelliSense и параметры диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="62232-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="62232-145">В этой лаборатории описаны действия, необходимые для выполнения данной задачи в Visual Studio при использовании **обычные параметры разработки** коллекции.</span><span class="sxs-lookup"><span data-stu-id="62232-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="62232-146">При выборе другого набора параметров для среды разработки может быть отличия в этапах, которые следует принимать во внимание.</span><span class="sxs-lookup"><span data-stu-id="62232-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="62232-147">Упражнение 1: Создание нового проекта Web Forms</span><span class="sxs-lookup"><span data-stu-id="62232-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="62232-148">В этом упражнении вы создадите новый узел веб-форм в Visual Studio 2013 с помощью **One ASP.NET** единого проекта, которые позволят легко интегрировать компоненты веб-форм, MVC и веб-API в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="62232-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="62232-149">Затем исследовать создаваемого решения и определения его частей, и наконец вы увидите веб-сайта в действии.</span><span class="sxs-lookup"><span data-stu-id="62232-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="62232-150">Задача 1 – Создание нового сайта с помощью единый интерфейс ASP.NET</span><span class="sxs-lookup"><span data-stu-id="62232-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="62232-151">В этой задаче будет начать создание нового веб-узла в Visual Studio на основе **One ASP.NET** тип проекта.</span><span class="sxs-lookup"><span data-stu-id="62232-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="62232-152">**Один ASP.NET** объединяет все технологии ASP.NET и дает возможность комбинировать и сопоставлять их в случае необходимости.</span><span class="sxs-lookup"><span data-stu-id="62232-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="62232-153">Затем будет распознавать различные компоненты веб-форм, MVC и веб-API, находящихся рядом друг с другом в приложении.</span><span class="sxs-lookup"><span data-stu-id="62232-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="62232-154">Откройте **Visual Studio Express 2013 для Web** и выберите **файл | Создать проект...**  для запуска нового решения.</span><span class="sxs-lookup"><span data-stu-id="62232-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Создание нового проекта](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="62232-156">*Создание нового проекта*</span><span class="sxs-lookup"><span data-stu-id="62232-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="62232-157">В **новый проект** выберите **веб-приложение ASP.NET** под **Visual C# | Web** вкладку и убедитесь, что **.NET Framework 4.5** выбран.</span><span class="sxs-lookup"><span data-stu-id="62232-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="62232-158">Назовите проект *MyHybridSite*, выберите **расположение** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="62232-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Новый проект веб-приложения ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="62232-160">*Создание нового проекта веб-приложения ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="62232-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="62232-161">В **новый проект ASP.NET** выберите **Web Forms** шаблона и выберите **MVC** и **веб-API** параметры.</span><span class="sxs-lookup"><span data-stu-id="62232-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="62232-162">Кроме того, убедитесь, что **проверки подлинности** включен режим **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="62232-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="62232-163">Нажмите кнопку **ОК** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="62232-163">Click **OK** to continue.</span></span>

    ![Создание нового проекта с помощью шаблона веб-формы, включая компоненты веб-API и MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="62232-165">*Создание нового проекта с помощью шаблона веб-формы, включая компоненты веб-API и MVC*</span><span class="sxs-lookup"><span data-stu-id="62232-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="62232-166">Теперь можно просмотреть структуру создаваемого решения.</span><span class="sxs-lookup"><span data-stu-id="62232-166">You can now explore the structure of the generated solution.</span></span>

    ![Изучение созданного решения](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="62232-168">*Изучение созданного решения*</span><span class="sxs-lookup"><span data-stu-id="62232-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="62232-169">**Учетная запись:** эта папка содержит страницы веб-формы для регистрации, выполните вход и управление учетными записями пользователей приложения.</span><span class="sxs-lookup"><span data-stu-id="62232-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="62232-170">Эта папка будет добавлена при **отдельных учетных записей пользователей** выбран параметр проверки подлинности во время настройки шаблона проекта веб-форм.</span><span class="sxs-lookup"><span data-stu-id="62232-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="62232-171">**Модели:** эта папка будет содержать классы, представляющие данные приложения.</span><span class="sxs-lookup"><span data-stu-id="62232-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="62232-172">**Контроллеры** и **представления**: этих папок являются обязательными для **ASP.NET MVC** и **веб-API ASP.NET** компонентов.</span><span class="sxs-lookup"><span data-stu-id="62232-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="62232-173">Вы исследуете технологии MVC и веб-API при следующем выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="62232-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="62232-174">**Default.aspx**, **Contact.aspx** и **About.aspx** файлы, предварительно определенные страницы веб-форм, которые можно использовать в качестве начальных точек для создания страниц, относящиеся к вашей приложение.</span><span class="sxs-lookup"><span data-stu-id="62232-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="62232-175">Программная логика этих файлов находится в отдельном файле называют &quot;кода&quot; файл, который имеет &quot;. aspx.vb&quot; или &quot;. aspx.cs&quot; расширения (в зависимости от язык, используемый).</span><span class="sxs-lookup"><span data-stu-id="62232-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="62232-176">Логику кода выполняется на сервере и динамически создает выходные данные HTML для страницы.</span><span class="sxs-lookup"><span data-stu-id="62232-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="62232-177">**Site.Master** и **Site.Mobile.Master** страницы определяют внешний вид и стандартное поведение всех страниц в приложении.</span><span class="sxs-lookup"><span data-stu-id="62232-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="62232-178">Дважды щелкните **Default.aspx** файл для просмотра содержимого страницы.</span><span class="sxs-lookup"><span data-stu-id="62232-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Изучение страницу Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="62232-180">*Изучение страницу Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="62232-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-181">**Страницы** директивы в начало файла определяет атрибуты, страницы веб-форм.</span><span class="sxs-lookup"><span data-stu-id="62232-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="62232-182">Например **MasterPageFile** атрибут задает путь к главному страницы — в этом случае *Site.Master* страниц и **Inherits** определяет атрибут класса с выделенным кодом для страницы для наследования.</span><span class="sxs-lookup"><span data-stu-id="62232-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="62232-183">Этот класс находится в файле определяется **CodeBehind** атрибута.</span><span class="sxs-lookup"><span data-stu-id="62232-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="62232-184">**Asp: Content** управления содержит фактическое содержимое страницы (текст, разметку и элементы управления) и сопоставляется с **asp: ContentPlaceHolder** управления на главной странице.</span><span class="sxs-lookup"><span data-stu-id="62232-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="62232-185">В этом случае будет отображено содержимое страницы внутри *тегу* элемент управления, определенный в *Site.Master* страницы.</span><span class="sxs-lookup"><span data-stu-id="62232-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="62232-186">Разверните **приложения\_запустить** папки и обратите внимание, **WebApiConfig.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="62232-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="62232-187">Visual Studio этот файл включен в состав созданный решения вы включили веб-API при настройке проекта с помощью шаблона One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62232-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="62232-188">Откройте **WebApiConfig.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="62232-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="62232-189">В *WebApiConfig* вы найдете конфигурации, связанные с веб-API, который сопоставляет HTTP класс направляет в **контроллеров веб-API**.</span><span class="sxs-lookup"><span data-stu-id="62232-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="62232-190">Откройте **RouteConfig.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="62232-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="62232-191">Внутри *RegisterRoutes* вы найдете конфигурации, связанной с MVC, который соответствует маршруту HTTP в метод **контроллеров MVC**.</span><span class="sxs-lookup"><span data-stu-id="62232-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="62232-192">Задача 2 – запуск решения</span><span class="sxs-lookup"><span data-stu-id="62232-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="62232-193">В этой задаче будет запустите созданный решение, исследовать приложения, а также некоторые функции, например перезаписи URL-адресов и встроенной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="62232-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="62232-194">Чтобы запустить решение, нажмите клавиши **F5** или нажмите кнопку **запустить** кнопку, расположенную на панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="62232-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="62232-195">Домашняя страница приложения следует открывать в браузере.</span><span class="sxs-lookup"><span data-stu-id="62232-195">The application home page should open in the browser.</span></span>

    ![Запуск решения](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="62232-197">Проверьте, вызываемого страниц веб-форм.</span><span class="sxs-lookup"><span data-stu-id="62232-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="62232-198">Чтобы сделать это, добавьте **/contact.aspx** URL-адрес в адресной строке и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="62232-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Понятные URL-адреса](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="62232-200">*Понятные URL-адреса*</span><span class="sxs-lookup"><span data-stu-id="62232-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-201">Как видите, URL-адрес примет **или обратитесь**.</span><span class="sxs-lookup"><span data-stu-id="62232-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="62232-202">Начиная с **ASP.NET 4**, были добавлены возможности маршрутизации URL-адрес веб-форм, можно создать URL-адреса, например  *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)*  вместо  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="62232-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="62232-203">Дополнительные сведения см. в [маршрутизации URL-адрес](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="62232-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="62232-204">Теперь вы исследуете поток проверки подлинности, который интегрируется в приложение.</span><span class="sxs-lookup"><span data-stu-id="62232-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="62232-205">Чтобы сделать это, нажмите кнопку **зарегистрировать** в правом верхнем углу страницы.</span><span class="sxs-lookup"><span data-stu-id="62232-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Регистрация нового пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="62232-207">*Регистрация нового пользователя*</span><span class="sxs-lookup"><span data-stu-id="62232-207">*Registering a new user*</span></span>
4. <span data-ttu-id="62232-208">В **зарегистрировать** введите **имя пользователя** и **пароль**, а затем нажмите кнопку **зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="62232-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Страница «Регистрация»](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="62232-210">*Страница «Регистрация»*</span><span class="sxs-lookup"><span data-stu-id="62232-210">*Register page*</span></span>
5. <span data-ttu-id="62232-211">Приложение регистрирует новую учетную запись, и пользователь прошел проверку.</span><span class="sxs-lookup"><span data-stu-id="62232-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Пользователь прошел проверку подлинности](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="62232-213">*Пользователь прошел проверку подлинности*</span><span class="sxs-lookup"><span data-stu-id="62232-213">*User authenticated*</span></span>
6. <span data-ttu-id="62232-214">Вернитесь в Visual Studio и нажмите клавишу **SHIFT + F5** остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="62232-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="62232-215">Упражнение 2: Создание контроллер MVC с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="62232-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="62232-216">В этом упражнении будет воспользоваться преимуществами платформы формирование шаблонов ASP.NET, предоставляемые Visual Studio для создания контроллера ASP.NET MVC 5 с действиями и представлениями Razor для выполнения операций CRUD без написания ни единой строки кода.</span><span class="sxs-lookup"><span data-stu-id="62232-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="62232-217">В процессе формирования шаблонов будет использоваться Entity Framework Code First для создания контекста данных и схему базы данных в базе данных SQL.</span><span class="sxs-lookup"><span data-stu-id="62232-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="62232-218">**Сначала о Entity Framework код**</span><span class="sxs-lookup"><span data-stu-id="62232-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="62232-219">Entity Framework (EF) является объектно реляционного сопоставления (ORM), который позволяет создавать приложения для доступа к данным путем программирования с концептуальной моделью приложения а не напрямую с помощью реляционной схеме хранения.</span><span class="sxs-lookup"><span data-stu-id="62232-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="62232-220">Процесс моделирования Entity Framework Code First можно использовать собственный домен классы для представления модели, использующая EF при выполнении запросов, функции отслеживания изменений и обновление.</span><span class="sxs-lookup"><span data-stu-id="62232-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="62232-221">С помощью Code First рабочего процесса разработки, вы не обязательно должны начинаться приложения путем создания базы данных или указание схемы.</span><span class="sxs-lookup"><span data-stu-id="62232-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="62232-222">Вместо этого можно написать стандартных классов .NET, определяющие наиболее подходящий объектами модели домена приложения и Entity Framework будет создана база данных автоматически.</span><span class="sxs-lookup"><span data-stu-id="62232-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="62232-223">Дополнительные сведения об Entity Framework [здесь](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="62232-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="62232-224">Задача 1 – Создание новой модели</span><span class="sxs-lookup"><span data-stu-id="62232-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="62232-225">Теперь мы определим **лицо** класс, который будет использовать в процессе формирования шаблонов для создания контроллера MVC и представления, модель.</span><span class="sxs-lookup"><span data-stu-id="62232-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="62232-226">Начнем с создания **лицо** класс модели и операций CRUD в контроллере автоматически создается с помощью функции формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="62232-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="62232-227">Откройте **Visual Studio Express 2013 для Web** и **MyHybridSite.sln** решение находится в **источника/Ex2-MvcScaffolding/начало** папки.</span><span class="sxs-lookup"><span data-stu-id="62232-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="62232-228">Кроме того можно продолжить с решением, полученный в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="62232-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="62232-229">В **обозревателе решений**, щелкните правой кнопкой мыши **моделей** папки **MyHybridSite** проект и выберите **добавить | Класс...** .</span><span class="sxs-lookup"><span data-stu-id="62232-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Добавление класса Person модели](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="62232-231">*Добавление класса Person модели*</span><span class="sxs-lookup"><span data-stu-id="62232-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="62232-232">В **Добавление нового элемента** диалоговое окно, имя файла *Person.cs* и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="62232-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Создание класса модели лица](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="62232-234">*Создание класса модели лица*</span><span class="sxs-lookup"><span data-stu-id="62232-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="62232-235">Замените содержимое **Person.cs** файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="62232-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="62232-236">Нажмите клавишу **сочетание клавиш CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="62232-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="62232-237">(Фрагмент - кода *PersonClass BringingTogetherOneAspNet - Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="62232-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="62232-238">В **обозревателе решений**, щелкните правой кнопкой мыши **MyHybridSite** проект и выберите **построения**, или нажмите клавишу **CTRL + SHIFT + B** для сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="62232-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="62232-239">Задача 2 – Создание контроллер MVC</span><span class="sxs-lookup"><span data-stu-id="62232-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="62232-240">Теперь, когда **лицо** модель создана, вы будете использовать формирование шаблонов ASP.NET MVC с Entity Framework для создания действия CRUD контроллера и представления для **лицо**.</span><span class="sxs-lookup"><span data-stu-id="62232-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="62232-241">В **обозревателе решений**, щелкните правой кнопкой мыши **контроллеров** папки **MyHybridSite** проект и выберите **добавить | Новый элемент формирования шаблонов...** .</span><span class="sxs-lookup"><span data-stu-id="62232-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Создание нового контроллера формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="62232-243">*Создание нового контроллера формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="62232-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="62232-244">В **Добавление формирования шаблонов** выберите **контроллер MVC 5 с представлениями, использующий Entity Framework** и нажмите кнопку **Add.**</span><span class="sxs-lookup"><span data-stu-id="62232-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![При выборе контроллер MVC 5 с представлениями и Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="62232-246">*При выборе контроллер MVC 5 с представлениями и Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="62232-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="62232-247">Задать *MvcPersonController* как **имя контроллера**выберите **использовать асинхронные действия контроллера** и выберите **лица (MyHybridSite.Models)**  как **класс модели**.</span><span class="sxs-lookup"><span data-stu-id="62232-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Добавление контроллера MVC с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="62232-249">*Добавление контроллера MVC с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="62232-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="62232-250">В разделе **класс контекста данных**, нажмите кнопку **новый контекст данных...** .</span><span class="sxs-lookup"><span data-stu-id="62232-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Создание нового контекста данных](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="62232-252">*Создание нового контекста данных*</span><span class="sxs-lookup"><span data-stu-id="62232-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="62232-253">В **новый контекст данных** диалоговом имя новому контексту данных *PersonContext* и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="62232-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Создание нового PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="62232-255">*Создание нового типа PersonContext*</span><span class="sxs-lookup"><span data-stu-id="62232-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="62232-256">Нажмите кнопку **добавить** для создания нового контроллера для **лицо** с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="62232-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="62232-257">Visual Studio создаст действия контроллера, контекст пользователя данных и представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="62232-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![После создания контроллер MVC с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="62232-259">*После создания контроллер MVC с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="62232-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="62232-260">Откройте **MvcPersonController.cs** файла в **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="62232-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="62232-261">Обратите внимание, что методы действия CRUD были созданы автоматически.</span><span class="sxs-lookup"><span data-stu-id="62232-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="62232-262">Выбрав **использовать асинхронные действия контроллера** флажок из формирование шаблонов параметров в предыдущих шагах, Visual Studio создает асинхронные методы действия для всех действий, для которых требуется доступ к контексту данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="62232-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="62232-263">Рекомендуется использовать асинхронные методы действия для долго выполняющихся, не связаны с ЦП запросами, чтобы избежать блокировки веб-сервера от выполнения операций во время обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="62232-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="62232-264">Задача 3 – Запуск решения</span><span class="sxs-lookup"><span data-stu-id="62232-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="62232-265">В этой задаче будет выполняться представления для решения еще раз, чтобы убедиться, что **лицо** работают надлежащим образом.</span><span class="sxs-lookup"><span data-stu-id="62232-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="62232-266">Будет добавлен новый пользователь, чтобы проверить, успешно сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="62232-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="62232-267">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="62232-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="62232-268">Перейдите к **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="62232-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="62232-269">Появится представление формирования шаблонов, показывающий список людей.</span><span class="sxs-lookup"><span data-stu-id="62232-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="62232-270">Нажмите кнопку **создать новый** для добавления нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="62232-270">Click **Create New** to add a new person.</span></span>

    ![Переход к формирования шаблонов представлений MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="62232-272">*Переход к формирования шаблонов представлений MVC*</span><span class="sxs-lookup"><span data-stu-id="62232-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="62232-273">В **создать** просмотреть, укажите **имя** и **возраст** человека и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="62232-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Добавление нового пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="62232-275">*Добавление нового пользователя*</span><span class="sxs-lookup"><span data-stu-id="62232-275">*Adding a new person*</span></span>
5. <span data-ttu-id="62232-276">Новый пользователь будет добавлен в список.</span><span class="sxs-lookup"><span data-stu-id="62232-276">The new person is added to the list.</span></span> <span data-ttu-id="62232-277">В списке элементов выберите **сведения** для отображения представления сведений пользователя.</span><span class="sxs-lookup"><span data-stu-id="62232-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="62232-278">Затем в **сведения** щелкните **списка** чтобы вернуться к представлению списка.</span><span class="sxs-lookup"><span data-stu-id="62232-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Представление сведений пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="62232-280">*Представление сведений пользователя*</span><span class="sxs-lookup"><span data-stu-id="62232-280">*Person's details view*</span></span>
6. <span data-ttu-id="62232-281">Нажмите кнопку **удалить** ссылку, чтобы удалить пользователя.</span><span class="sxs-lookup"><span data-stu-id="62232-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="62232-282">В **удаление** щелкните **удаление** для подтверждения операции.</span><span class="sxs-lookup"><span data-stu-id="62232-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Удаление пользователя](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="62232-284">*Удаление пользователя*</span><span class="sxs-lookup"><span data-stu-id="62232-284">*Deleting a person*</span></span>
7. <span data-ttu-id="62232-285">Вернитесь в Visual Studio и нажмите клавишу **SHIFT + F5** остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="62232-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="62232-286">Упражнение 3: Создание контроллера Web API, с помощью формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="62232-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="62232-287">Платформа веб-API является частью стека ASP.NET и предназначены для упрощения реализации службы HTTP, как правило, отправки и получения данных в формате JSON или XML через RESTful API.</span><span class="sxs-lookup"><span data-stu-id="62232-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="62232-288">В этом упражнении будет использовать формирование шаблонов ASP.NET попытку создать контроллер веб-API.</span><span class="sxs-lookup"><span data-stu-id="62232-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="62232-289">Применяются те же **лицо** и **PersonContext** классы из предыдущего упражнения для предоставления данных того же пользователя в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="62232-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="62232-290">Вы увидите, как можно предоставить те же ресурсы по-разному, в то же приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62232-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="62232-291">Задача 1 – Создание контроллера Web API</span><span class="sxs-lookup"><span data-stu-id="62232-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="62232-292">В этой задаче вы создадите новый **контроллер Web API** , будет представлять данные пользователя в машину потребляемые формат JSON.</span><span class="sxs-lookup"><span data-stu-id="62232-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="62232-293">Если еще не открыт, откройте **Visual Studio Express 2013 для Web** и откройте **MyHybridSite.sln** решение находится в **источника/Ex3-WebAPI/начало** папки.</span><span class="sxs-lookup"><span data-stu-id="62232-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="62232-294">Кроме того можно продолжить с решением, полученный в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="62232-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-295">Если вы запустите решение Begin с Упражнение 3, нажмите клавиши **CTRL + SHIFT + B** для построения решения.</span><span class="sxs-lookup"><span data-stu-id="62232-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="62232-296">В **обозревателе решений**, щелкните правой кнопкой мыши **контроллеров** папки **MyHybridSite** проект и выберите **добавить | Новый элемент формирования шаблонов...** .</span><span class="sxs-lookup"><span data-stu-id="62232-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Создание нового контроллера формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="62232-298">*Создание нового контроллера формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="62232-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="62232-299">В **Добавление формирования шаблонов** выберите **веб-API** в левой панели, затем **Web API 2 контроллер с действиями, использующий Entity Framework** в средней области и нажмите кнопку  **Добавите.**</span><span class="sxs-lookup"><span data-stu-id="62232-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="62232-300">![При выборе 2 контроллер Web API с действиями и Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "при выборе Web API 2 контроллер с действиями и Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="62232-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="62232-301">*Выбор контроллера Web API 2 с действиями и Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="62232-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="62232-302">Задать *ApiPersonController* как **имя контроллера**выберите **использовать асинхронные действия контроллера** и выберите **лица (MyHybridSite.Models)**  и **PersonContext (MyHybridSite.Models)** как **модель** и **контекст данных** классы соответственно.</span><span class="sxs-lookup"><span data-stu-id="62232-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="62232-303">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="62232-303">Then click **Add**.</span></span>

    <span data-ttu-id="62232-304">![Добавление контроллера Web API с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Добавление контроллера веб-API с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="62232-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="62232-305">*Добавление контроллера веб-API с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="62232-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="62232-306">Visual Studio создаст **ApiPersonController** класс с четырьмя действиями CRUD для работы с данными.</span><span class="sxs-lookup"><span data-stu-id="62232-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="62232-307">![После создания контроллера веб-API с помощью формирования шаблонов](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "после создания контроллера веб-API с помощью формирования шаблонов")</span><span class="sxs-lookup"><span data-stu-id="62232-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="62232-308">*После создания контроллера веб-API с помощью формирования шаблонов*</span><span class="sxs-lookup"><span data-stu-id="62232-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="62232-309">Откройте **ApiPersonController.cs** файл и проверьте *GetPeople* метода действия.</span><span class="sxs-lookup"><span data-stu-id="62232-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="62232-310">Этот метод отправляет запрос в поле db **PersonContext** тип для получения данных пользователей.</span><span class="sxs-lookup"><span data-stu-id="62232-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="62232-311">Обратите внимание, комментарий выше определении метода.</span><span class="sxs-lookup"><span data-stu-id="62232-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="62232-312">Он предоставляет URI, который представляет это действие, который будет использоваться в следующей задаче.</span><span class="sxs-lookup"><span data-stu-id="62232-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="62232-313">По умолчанию должен перехватывать запросы к веб-API */api* путь для предотвращения конфликтов с контроллеров MVC.</span><span class="sxs-lookup"><span data-stu-id="62232-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="62232-314">Если необходимо изменить эту настройку, см. [маршрутизации в ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="62232-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="62232-315">Задача 2 – запуск решения</span><span class="sxs-lookup"><span data-stu-id="62232-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="62232-316">В этой задаче будут использованы в Internet Explorer **средств разработчика F12** для проверки полного ответа от контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="62232-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="62232-317">Вы увидите, как можно записывать сетевой трафик, чтобы получить больше возможностей регулировать данные приложения.</span><span class="sxs-lookup"><span data-stu-id="62232-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="62232-318">Убедитесь, что **Internet Explorer** выбран в **запустить** кнопку, расположенную на панели инструментов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="62232-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Параметр Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="62232-320">**Средств разработчика F12** имеют широкий набор функциональных возможностей, не включенные в это практическая работа.</span><span class="sxs-lookup"><span data-stu-id="62232-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="62232-321">Если вы хотите узнать больше о ней, см. [с помощью средств разработчика F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="62232-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="62232-322">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="62232-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-323">Чтобы правильно выполнить эту задачу, приложение должно иметь данные.</span><span class="sxs-lookup"><span data-stu-id="62232-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="62232-324">Если базы данных пуст, можно вернуться к задаче 3 в упражнении 2 и выполните действия по созданию нового пользователя с помощью представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="62232-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="62232-325">В браузере, нажмите клавишу **F12** Открытие **средств разработчика** панель.</span><span class="sxs-lookup"><span data-stu-id="62232-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="62232-326">Нажмите клавишу **CTRL** + **4** или нажмите кнопку **сети** значок и нажмите кнопку с зеленой стрелкой, чтобы начать отслеживать сетевой трафик.</span><span class="sxs-lookup"><span data-stu-id="62232-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="62232-327">![Запуск записи сетевого трафика веб-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "записи сетевого трафика запуск веб-API")</span><span class="sxs-lookup"><span data-stu-id="62232-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="62232-328">*Запуск записи сетевого трафика веб-API*</span><span class="sxs-lookup"><span data-stu-id="62232-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="62232-329">Добавление **api/ApiPerson** URL-адрес в адресной строке браузера.</span><span class="sxs-lookup"><span data-stu-id="62232-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="62232-330">Теперь будет проверять сведения об ответе от **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="62232-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="62232-331">![Получение данных пользователя с помощью веб-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "получение данных пользователя с помощью веб-API")</span><span class="sxs-lookup"><span data-stu-id="62232-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="62232-332">*Получение данных по продажам через веб-API*</span><span class="sxs-lookup"><span data-stu-id="62232-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-333">После завершения загрузки, будет предложено задать действие с помощью загруженного файла.</span><span class="sxs-lookup"><span data-stu-id="62232-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="62232-334">Не закрывайте диалоговое окно для просмотра содержимого ответа с помощью окна инструментов для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="62232-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="62232-335">Теперь будет проверять текст ответа.</span><span class="sxs-lookup"><span data-stu-id="62232-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="62232-336">Чтобы сделать это, нажмите кнопку **сведения** и нажмите кнопку **текст ответа**.</span><span class="sxs-lookup"><span data-stu-id="62232-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="62232-337">Можно проверить, что загруженные данные приведен список объектов со свойствами **идентификатор**, **имя** и **возраст** , которые соответствуют **лицо** класс.</span><span class="sxs-lookup"><span data-stu-id="62232-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="62232-338">![Просмотр веб-текст ответа API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "просмотра веб-текст ответа API")</span><span class="sxs-lookup"><span data-stu-id="62232-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="62232-339">*Текст ответа API просмотра Web*</span><span class="sxs-lookup"><span data-stu-id="62232-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="62232-340">Задача 3 – Добавление веб-страницы справки API</span><span class="sxs-lookup"><span data-stu-id="62232-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="62232-341">При создании веб-API, полезно создать страницу справки, чтобы другие разработчики знали способ вызова API.</span><span class="sxs-lookup"><span data-stu-id="62232-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="62232-342">Можно создать и вручную обновить страницы документации, но лучше автоматически создавать их, чтобы избежать необходимости делать работ по обслуживанию.</span><span class="sxs-lookup"><span data-stu-id="62232-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="62232-343">В этой задаче используется пакет Nuget для автоматического создания страниц справки веб-API в решение.</span><span class="sxs-lookup"><span data-stu-id="62232-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="62232-344">Из **средства** в Visual Studio, выберите пункт меню **диспетчер пакетов библиотеки**, а затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="62232-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="62232-345">В **консоль диспетчера пакетов** окно, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="62232-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="62232-346">**Microsoft.AspNet.WebApi.HelpPage** пакет устанавливает необходимые сборки и добавляет представлений MVC для страницы справки, в разделе **областей или HelpPage** папки.</span><span class="sxs-lookup"><span data-stu-id="62232-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="62232-347">![Область HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage области")</span><span class="sxs-lookup"><span data-stu-id="62232-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="62232-348">*HelpPage области*</span><span class="sxs-lookup"><span data-stu-id="62232-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="62232-349">По умолчанию с помощью страницы имеют заполнитель строки для документации.</span><span class="sxs-lookup"><span data-stu-id="62232-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="62232-350">Комментарии XML-документации можно использовать для создания документации.</span><span class="sxs-lookup"><span data-stu-id="62232-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="62232-351">Чтобы включить эту функцию, откройте **HelpPageConfig.cs** файл, расположенный в **HelpPage области приложений и\_запустить** папки и раскомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="62232-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="62232-352">В **обозревателе решений**, щелкните правой кнопкой мыши проект **MyHybridSite**выберите **свойства** и нажмите кнопку **построения** вкладки.</span><span class="sxs-lookup"><span data-stu-id="62232-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="62232-353">![Вкладка Построение](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "построения раздела")</span><span class="sxs-lookup"><span data-stu-id="62232-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="62232-354">*Вкладка Построение*</span><span class="sxs-lookup"><span data-stu-id="62232-354">*Build tab*</span></span>
5. <span data-ttu-id="62232-355">В разделе **вывода**выберите **XML-файл документации**.</span><span class="sxs-lookup"><span data-stu-id="62232-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="62232-356">В поле ввода введите **приложения\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="62232-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="62232-357">![Вывод раздела вкладка построение](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "вывода раздела вкладка построение")</span><span class="sxs-lookup"><span data-stu-id="62232-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="62232-358">*Выходные данные раздела вкладка построение*</span><span class="sxs-lookup"><span data-stu-id="62232-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="62232-359">Нажмите клавишу **CTRL** + **S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="62232-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="62232-360">Откройте **ApiPersonController.cs** файл из **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="62232-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="62232-361">Введите новую строку между *GetPeople* сигнатуру метода и */ / api/ApiPerson GET* комментарий, а затем введите три косые черты.</span><span class="sxs-lookup"><span data-stu-id="62232-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-362">Visual Studio автоматически добавляет XML-элементы, которые определяют документацию по методу.</span><span class="sxs-lookup"><span data-stu-id="62232-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="62232-363">Добавьте текст сводки и возвращаемое значение для *GetPeople* метод.</span><span class="sxs-lookup"><span data-stu-id="62232-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="62232-364">Он должен выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="62232-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="62232-365">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="62232-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="62232-366">Добавление **/help** URL-адрес в адресной строке, чтобы перейти к странице справки.</span><span class="sxs-lookup"><span data-stu-id="62232-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="62232-367">![Страница справки веб-API ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "страница справки веб-API ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="62232-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="62232-368">*Страница справки веб-API ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="62232-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="62232-369">Основное содержимое страницы — это таблица, API-функций, сгруппированных по контроллера.</span><span class="sxs-lookup"><span data-stu-id="62232-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="62232-370">Записи таблицы создаются динамически с помощью **IApiExplorer** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="62232-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="62232-371">Если добавить или обновить контроллер API, таблицы автоматически обновляется при очередном построения приложения.</span><span class="sxs-lookup"><span data-stu-id="62232-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="62232-372">**API** столбец содержит метод HTTP и относительного URI.</span><span class="sxs-lookup"><span data-stu-id="62232-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="62232-373">**Описание** столбец содержит сведения, которые были извлечены из документации по данному методу.</span><span class="sxs-lookup"><span data-stu-id="62232-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="62232-374">Обратите внимание, что описание, которое вы добавили над определением метода отображается в столбце «Описание».</span><span class="sxs-lookup"><span data-stu-id="62232-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="62232-375">![Описание метода API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "описание метода API")</span><span class="sxs-lookup"><span data-stu-id="62232-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="62232-376">*Описание метода API*</span><span class="sxs-lookup"><span data-stu-id="62232-376">*API method description*</span></span>
13. <span data-ttu-id="62232-377">Выберите один из методов API, чтобы перейти на страницу с более подробными сведениями, включая примеры текстов ответа.</span><span class="sxs-lookup"><span data-stu-id="62232-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="62232-378">![Сведения о странице сведений о](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "страницу подробных сведений")</span><span class="sxs-lookup"><span data-stu-id="62232-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="62232-379">*Страница подробные сведения*</span><span class="sxs-lookup"><span data-stu-id="62232-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="62232-380">Сводка</span><span class="sxs-lookup"><span data-stu-id="62232-380">Summary</span></span>

<span data-ttu-id="62232-381">Выполнив этой практической вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="62232-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="62232-382">Создание нового веб-приложения, используя единый интерфейс ASP.NET в Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="62232-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="62232-383">Интегрировать несколько технологий ASP.NET в один отдельный проект</span><span class="sxs-lookup"><span data-stu-id="62232-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="62232-384">Создать из классов модели с помощью формирования шаблонов ASP.NET MVC контроллеры и представления</span><span class="sxs-lookup"><span data-stu-id="62232-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="62232-385">Создание контроллеров веб-API, которые используют функции, такие как асинхронного программирования и доступа к данным через Entity Framework</span><span class="sxs-lookup"><span data-stu-id="62232-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="62232-386">Автоматически создавать веб-страницы справки API контроллеров</span><span class="sxs-lookup"><span data-stu-id="62232-386">Automatically generate Web API Help Pages for your controllers</span></span>
