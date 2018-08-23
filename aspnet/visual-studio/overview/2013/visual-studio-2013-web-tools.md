---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Практическое лабораторное занятие: Visual Studio 2013 веб-инструменты | Документация Майкрософт'
author: rick-anderson
description: Visual Studio — это среда разработки отлично для. Windows на основе .NET и веб-проектов. Он включает в себя мощный текстовый редактор, который можно легко использовать для...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 82248efd767c1110b9a4067b7d0c0e2ecafcbef9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838248"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="24927-104">Практическое лабораторное занятие: Visual Studio 2013 веб-инструменты</span><span class="sxs-lookup"><span data-stu-id="24927-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="24927-105">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="24927-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="24927-106">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="24927-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="24927-107">Visual Studio — это среда разработки отлично для. Windows на основе .NET и веб-проектов.</span><span class="sxs-lookup"><span data-stu-id="24927-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="24927-108">Он включает мощный текстовый редактор, который можно легко использовать для изменения отдельных файлов без проекта.</span><span class="sxs-lookup"><span data-stu-id="24927-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="24927-109">Visual Studio поддерживает дерево синтаксического анализа, полнофункциональную при редактировании каждого файла.</span><span class="sxs-lookup"><span data-stu-id="24927-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="24927-110">Это позволяет Visual Studio обеспечивают непревзойденную функция автозавершения и действия на основе документа во время процесса разработки более быстрым и стал более приятным.</span><span class="sxs-lookup"><span data-stu-id="24927-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="24927-111">Эти функции особенно выражены в документах HTML и CSS.</span><span class="sxs-lookup"><span data-stu-id="24927-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="24927-112">Все эти возможности также доступна для расширений, упрощая для расширения редакторам с более новыми мощными функциями в соответствии с потребностями.</span><span class="sxs-lookup"><span data-stu-id="24927-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="24927-113">Web Essentials является коллекцией (обычно) связанные с веб-расширений для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="24927-114">В нем много новые варианты завершения IntelliSense (особенно для CSS), новые возможности связи с браузером, автоматически файлы JSHint для JavaScript, появлению новых предупреждений для HTML, CSS и многие другие возможности, которые необходимы для разработки современных веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="24927-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="24927-115">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="24927-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="24927-116">Обзор</span><span class="sxs-lookup"><span data-stu-id="24927-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="24927-117">Цели</span><span class="sxs-lookup"><span data-stu-id="24927-117">Objectives</span></span>

<span data-ttu-id="24927-118">В этой практической работу вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="24927-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="24927-119">Используйте новые возможности редактора HTML, включенные в Web Essentials, например форматированного фрагменты кода HTML5 и Zen кодирования</span><span class="sxs-lookup"><span data-stu-id="24927-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="24927-120">Используйте новые возможности редактора CSS, включенные в Web Essentials, такие как палитра цветов и обозревателя матриц подсказки</span><span class="sxs-lookup"><span data-stu-id="24927-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="24927-121">Используйте новые возможности редактора JavaScript, включенные в Web Essentials, например извлечь файл, а также IntelliSense для всех элементов HTML</span><span class="sxs-lookup"><span data-stu-id="24927-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="24927-122">Обмен данными между обозревателем и Visual Studio с помощью привязывания к браузеру</span><span class="sxs-lookup"><span data-stu-id="24927-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="24927-123">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="24927-123">Prerequisites</span></span>

<span data-ttu-id="24927-124">Для завершения этой практической работу требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="24927-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="24927-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="24927-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="24927-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="24927-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="24927-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="24927-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="24927-128">Установка</span><span class="sxs-lookup"><span data-stu-id="24927-128">Setup</span></span>

<span data-ttu-id="24927-129">Для выполнения упражнений в этой практической работу, необходимо сначала настроить среду.</span><span class="sxs-lookup"><span data-stu-id="24927-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="24927-130">Откройте окно проводника Windows и перейдите к лаборатории **источника** папки.</span><span class="sxs-lookup"><span data-stu-id="24927-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="24927-131">Щелкните правой кнопкой мыши **Setup.cmd** и выберите **Запуск от имени администратора** для запуска процесса установки, будет настроить среду и установить фрагменты кода Visual Studio для этой лабораторной работы.</span><span class="sxs-lookup"><span data-stu-id="24927-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="24927-132">Если отображается диалоговое окно контроля учетных записей, подтвердите действие для продолжения.</span><span class="sxs-lookup"><span data-stu-id="24927-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="24927-133">Убедитесь, что все зависимости для этой лабораторной работы вы вернули перед запуском программы установки.</span><span class="sxs-lookup"><span data-stu-id="24927-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="24927-134">Фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="24927-134">Using the Code Snippets</span></span>

<span data-ttu-id="24927-135">В этом документе лаборатории вам будет рекомендовано вставлять блоки кода.</span><span class="sxs-lookup"><span data-stu-id="24927-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="24927-136">Для удобства основная часть кода предоставляется как фрагменты кода Visual Studio, к которому можно получить из в Visual Studio 2013, чтобы избежать необходимости добавлять ее вручную.</span><span class="sxs-lookup"><span data-stu-id="24927-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="24927-137">Каждого упражнения сопровождается начальный решения, расположенный в **начать** папку упражнения, чему вы сможете каждого упражнения, независимо от других.</span><span class="sxs-lookup"><span data-stu-id="24927-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="24927-138">Имейте в виду, что фрагменты кода, которые добавляются во время атаки, отсутствуют эти стартовые решения и могут не работать, пока не будут выполнены упражнения.</span><span class="sxs-lookup"><span data-stu-id="24927-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="24927-139">В исходном коде для упражнения, вы также найдете **окончания** папку, содержащую решение Visual Studio с кодом, полученный в результате выполнения действий, описанных в соответствующий упражнении.</span><span class="sxs-lookup"><span data-stu-id="24927-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="24927-140">Эти решения можно использовать в качестве руководства, если вам нужна дополнительная помощь, отвечая на это Практическое занятие.</span><span class="sxs-lookup"><span data-stu-id="24927-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="24927-141">Упражнения</span><span class="sxs-lookup"><span data-stu-id="24927-141">Exercises</span></span>

<span data-ttu-id="24927-142">Это Практическое занятие включает в себя следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="24927-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="24927-143">Работа с привязывание к браузеру и Web Essentials</span><span class="sxs-lookup"><span data-stu-id="24927-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="24927-144">Чтобы преимущества фрагменты кода IntelliSense</span><span class="sxs-lookup"><span data-stu-id="24927-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="24927-145">При первом запуске Visual Studio, необходимо выбрать один из предварительно определенных коллекций параметров.</span><span class="sxs-lookup"><span data-stu-id="24927-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="24927-146">Каждой коллекции предопределенных соответствует конкретному стилю разработки и определяет макетов окон, поведение редактора, фрагменты кода IntelliSense и параметры диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="24927-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="24927-147">В этом лабораторном занятии описаны действия, необходимые для выполнения данной задачи в Visual Studio при использовании **обычные параметры разработки** коллекции.</span><span class="sxs-lookup"><span data-stu-id="24927-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="24927-148">При выборе другого набора параметров для среды разработки, возможно, различия в действиях, которые следует учитывать.</span><span class="sxs-lookup"><span data-stu-id="24927-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="24927-149">Упражнение 1: Работа с привязывание к браузеру и Web Essentials</span><span class="sxs-lookup"><span data-stu-id="24927-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="24927-150">**Web Essentials** представляет собой расширение Visual Studio, которое добавляет ряд полезных возможностей для современных веб-разработки, главным образом сосредоточены на веб-разработки приложений, намного быстрее и стал более приятным.</span><span class="sxs-lookup"><span data-stu-id="24927-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="24927-151">Web Essentials можно установить из коллекции расширений в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="24927-152">**Связь с браузером** — это новая функция, включенные в Visual Studio 2013, которая предоставляет канал между Visual Studio IDE и любым открытым браузером для обмена данными между веб-приложения и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="24927-153">Web Essentials расширяет возможности связи с браузером с помощью средств для управления объектной модели DOM и стили CSS веб-страниц непосредственно из браузера.</span><span class="sxs-lookup"><span data-stu-id="24927-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="24927-154">В этом упражнении будут рассматриваться некоторые возможности, поддерживаемые **Web Essentials** и **связь с браузером** для улучшения страницы простой тест.</span><span class="sxs-lookup"><span data-stu-id="24927-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="24927-155">Задача 1 - Запуск проекта в различных браузерах</span><span class="sxs-lookup"><span data-stu-id="24927-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="24927-156">В этой задаче вы настроите веб-приложения для запуска в нескольких браузерах одновременно, что полезно для тестирования обозреватели.</span><span class="sxs-lookup"><span data-stu-id="24927-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="24927-157">Откройте **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="24927-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="24927-158">В **файл** меню, выберите **Open | Проект или решение...**  и перейдите к **сервера Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** в **источника** папку лаборатории (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="24927-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="24927-159">Выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="24927-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="24927-160">На панели инструментов Visual Studio разверните меню браузера и выберите **просмотр с помощью...** .</span><span class="sxs-lookup"><span data-stu-id="24927-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="24927-161">![Просмотр с помощью пункта меню](visual-studio-2013-web-tools/_static/image1.png "Обзор с меню браузера")</span><span class="sxs-lookup"><span data-stu-id="24927-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="24927-162">*Просмотр с помощью пункта меню*</span><span class="sxs-lookup"><span data-stu-id="24927-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="24927-163">В **просмотр с помощью** диалоговом окне выберите **Google Chrome** и **Internet Explorer** , удерживая нажатой **CTRL** ключа и нажмите кнопку  **По умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="24927-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="24927-164">![Обзор с диалоговым окном](visual-studio-2013-web-tools/_static/image2.png "Обзор с диалоговым окном")</span><span class="sxs-lookup"><span data-stu-id="24927-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="24927-165">*Выберите несколько браузеров по умолчанию*</span><span class="sxs-lookup"><span data-stu-id="24927-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="24927-166">Google Chrome и Internet Explorer теперь должен представляться как браузеры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="24927-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="24927-167">Нажмите кнопку **отменить** чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="24927-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="24927-168">![Google Chrome и Internet Explorer как браузеры по умолчанию](visual-studio-2013-web-tools/_static/image3.png "Google Chrome и Internet Explorer браузеры по умолчанию")</span><span class="sxs-lookup"><span data-stu-id="24927-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="24927-169">*Google Chrome и Internet Explorer как браузеры по умолчанию*</span><span class="sxs-lookup"><span data-stu-id="24927-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-170">После настройки в качестве браузера по умолчанию **несколько браузеров** установлен флажок в меню обозревателя.</span><span class="sxs-lookup"><span data-stu-id="24927-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="24927-171">![Несколько браузеров](visual-studio-2013-web-tools/_static/image4.png "несколько браузеров")</span><span class="sxs-lookup"><span data-stu-id="24927-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="24927-172">Нажмите клавишу **CTRL** + **F5** для запуска приложения без отладки.</span><span class="sxs-lookup"><span data-stu-id="24927-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="24927-173">При открытии оба окна браузера, установите один из них над другим для просмотра обновлений на обоих браузерах одновременно.</span><span class="sxs-lookup"><span data-stu-id="24927-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="24927-174">Обозреватели должны отображать веб-страницы с голубой прямоугольник.</span><span class="sxs-lookup"><span data-stu-id="24927-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="24927-175">![Поместив один браузер над другим](visual-studio-2013-web-tools/_static/image5.png "размещение один браузер над другим")</span><span class="sxs-lookup"><span data-stu-id="24927-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="24927-176">*Поместив один браузер над другим*</span><span class="sxs-lookup"><span data-stu-id="24927-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="24927-177">Не закрывайте браузеров.</span><span class="sxs-lookup"><span data-stu-id="24927-177">Do not close the browsers.</span></span> <span data-ttu-id="24927-178">В следующей задаче будет использовать их.</span><span class="sxs-lookup"><span data-stu-id="24927-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="24927-179">Задача 2 - с помощью Zen кодирования для создания HTML-элементов</span><span class="sxs-lookup"><span data-stu-id="24927-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="24927-180">**Кодирование Zen** — это редактор кода подключаемого модуля для высокоскоростной HTML, XML, XSL (или любых других форматах, структурированный код) и редактирования.</span><span class="sxs-lookup"><span data-stu-id="24927-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="24927-181">Этот подключаемый модуль лежит механизм мощные сокращение, который позволяет расширить - аналогичную селекторов CSS - выражения в HTML-код.</span><span class="sxs-lookup"><span data-stu-id="24927-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="24927-182">Zen кодирования не позволяет быстро написать синтаксиса селектора стиля HTML, с помощью CSS.</span><span class="sxs-lookup"><span data-stu-id="24927-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="24927-183">В этом упражнении будет использовать функцию Zen кодирования, предоставляемые Web Essentials для создания кнопки HTML, представляющих параметры вопроса.</span><span class="sxs-lookup"><span data-stu-id="24927-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="24927-184">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="24927-185">Откройте **Index.cshtml** файл, расположенный в **представления** | **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="24927-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="24927-186">Замените **&lt;!--TODO: Добавьте здесь--параметры&gt;** комментарий со следующим кодом и нажмите клавишу **ВКЛАДКЕ**.</span><span class="sxs-lookup"><span data-stu-id="24927-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="24927-187">Код должен быть развернут в виде HTML.</span><span class="sxs-lookup"><span data-stu-id="24927-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="24927-188">![Расширен HTML](visual-studio-2013-web-tools/_static/image6.png "расширен HTML")</span><span class="sxs-lookup"><span data-stu-id="24927-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="24927-189">*Развернутое HTML*</span><span class="sxs-lookup"><span data-stu-id="24927-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-190">Дополнительные сведения о синтаксисе Zen кодирования, см. в следующих [статье](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="24927-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="24927-191">Нажмите кнопку **обновить подключенные браузеры** кнопку, чтобы обновить оба браузера.</span><span class="sxs-lookup"><span data-stu-id="24927-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="24927-192">![Обновить подключенные браузеры](visual-studio-2013-web-tools/_static/image7.png "обновить подключенные браузеры")</span><span class="sxs-lookup"><span data-stu-id="24927-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="24927-193">*Обновить подключенные браузеры*</span><span class="sxs-lookup"><span data-stu-id="24927-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="24927-194">![Internet Explorer — страница обновлена с четырьмя кнопками](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer — страница обновлена с четырьмя кнопками")</span><span class="sxs-lookup"><span data-stu-id="24927-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="24927-195">*Internet Explorer — страница обновлена с четырьмя кнопками*</span><span class="sxs-lookup"><span data-stu-id="24927-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="24927-196">![Google Chrome: страница обновлена с четырьмя кнопками](visual-studio-2013-web-tools/_static/image9.png "Google Chrome: страница обновлена с четырьмя кнопками")</span><span class="sxs-lookup"><span data-stu-id="24927-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="24927-197">*Google Chrome: страница обновлена с четырьмя кнопками*</span><span class="sxs-lookup"><span data-stu-id="24927-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="24927-198">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="24927-199">Кнопки добавления на страницу, но по-прежнему необходимо добавить вопрос шаблона.</span><span class="sxs-lookup"><span data-stu-id="24927-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="24927-200">Чтобы сделать это, будут использовать новую функцию в Web Essentials вызывается **генератор Lorem Ipsum**.</span><span class="sxs-lookup"><span data-stu-id="24927-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="24927-201">Найдите **div** элемент с **класс** атрибут **front**.</span><span class="sxs-lookup"><span data-stu-id="24927-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="24927-202">Добавьте следующий код в качестве первого дочернего элемента из **div**и нажмите клавишу **ВКЛАДКЕ**.</span><span class="sxs-lookup"><span data-stu-id="24927-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="24927-203">Код должен быть развернут в виде HTML.</span><span class="sxs-lookup"><span data-stu-id="24927-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="24927-204">![Автоматически созданное lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "автоматически сгенерированных Lorem Ipsum")</span><span class="sxs-lookup"><span data-stu-id="24927-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="24927-205">*Автоматически созданное lorem Ipsum*</span><span class="sxs-lookup"><span data-stu-id="24927-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-206">В процессе кодирования Zen можно приступить к созданию кода Lorem Ipsum непосредственно в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="24927-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="24927-207">Просто введите **lorem** и нажмите кнопку **ВКЛАДКЕ** и 30 объектов word Lorem Ipsum будет вставлен текст.</span><span class="sxs-lookup"><span data-stu-id="24927-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="24927-208">Например,</span><span class="sxs-lookup"><span data-stu-id="24927-208">E.g.</span></span> <span data-ttu-id="24927-209">*lorem10* вставляет 10 Lorem Ipsum слов.</span><span class="sxs-lookup"><span data-stu-id="24927-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="24927-210">Вы добавите эмблему в верхней части вопрос с помощью еще одна новая функция в Web Essentials вызывается **пикселей Lorem генератор**.</span><span class="sxs-lookup"><span data-stu-id="24927-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="24927-211">Добавьте следующий код в качестве первого дочернего элемента из **div** элемент с **контейнера** как **класс** и нажмите клавишу **ВКЛАДКЕ**.</span><span class="sxs-lookup"><span data-stu-id="24927-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="24927-212">Код должен развернуть в виде HTML.</span><span class="sxs-lookup"><span data-stu-id="24927-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="24927-213">![Автоматически созданное lorem пикселей](visual-studio-2013-web-tools/_static/image11.png "автоматически сгенерированных Lorem пикселей")</span><span class="sxs-lookup"><span data-stu-id="24927-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="24927-214">*Автоматически созданное lorem пикселей*</span><span class="sxs-lookup"><span data-stu-id="24927-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-215">В процессе кодирования Zen вы можно также создать пикселей Lorem код непосредственно в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="24927-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="24927-216">Просто введите **pix-200 x 200-животных** и нажмите кнопку **ВКЛАДКЕ** и **img** вставляется тег с 200 x 200 образа animal.</span><span class="sxs-lookup"><span data-stu-id="24927-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="24927-217">Дополнительные сведения см. [пикселей Lorem](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="24927-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="24927-218">Нажмите кнопку **обновить подключенные браузеры** кнопку, чтобы обновить оба браузера.</span><span class="sxs-lookup"><span data-stu-id="24927-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="24927-219">![Internet Explorer — автоматически созданное изображение и текст](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer — автоматически созданное изображение и текст")</span><span class="sxs-lookup"><span data-stu-id="24927-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="24927-220">*Internet Explorer — автоматически созданное изображение и текст*</span><span class="sxs-lookup"><span data-stu-id="24927-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="24927-221">![Google Chrome: автоматически созданное изображение и текст](visual-studio-2013-web-tools/_static/image13.png "Google Chrome: автоматически созданное изображение и текст")</span><span class="sxs-lookup"><span data-stu-id="24927-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="24927-222">*Google Chrome: автоматически созданное изображение и текст*</span><span class="sxs-lookup"><span data-stu-id="24927-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-223">Поскольку изображение выбирается случайным образом при добавлении фрагмента кода, изображения, отображаемого в браузерах может отличаться.</span><span class="sxs-lookup"><span data-stu-id="24927-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="24927-224">Не закрывайте браузеров.</span><span class="sxs-lookup"><span data-stu-id="24927-224">Do not close the browsers.</span></span> <span data-ttu-id="24927-225">В следующей задаче будет использовать их.</span><span class="sxs-lookup"><span data-stu-id="24927-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="24927-226">Задача 3 - обновление свойства стиля</span><span class="sxs-lookup"><span data-stu-id="24927-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="24927-227">В этой задаче вы воспользуетесь связь с браузером **проверить режим** возможность определить точное расположение, где формируется определенный элемент DOM, а затем обновите свойство цвета элемента с помощью средства выбора цвета, предоставляемые веб Essentials.</span><span class="sxs-lookup"><span data-stu-id="24927-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="24927-228">В обозревателе Internet Explorer, нажмите клавишу **CTRL** + **ALT** + **я** для перехода в режим проверки.</span><span class="sxs-lookup"><span data-stu-id="24927-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="24927-229">Наведите указатель на света синюю границу и нажмите кнопку.</span><span class="sxs-lookup"><span data-stu-id="24927-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="24927-230">![Наведя указатель света синюю границу](visual-studio-2013-web-tools/_static/image14.png "перемещении указателя над светлой темно-синяя рамка")</span><span class="sxs-lookup"><span data-stu-id="24927-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="24927-231">*Наведя указатель светлой темно-синяя рамка*</span><span class="sxs-lookup"><span data-stu-id="24927-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="24927-232">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="24927-233">Обратите внимание на то, как HTML-элемента, выбранного в обозревателе также выбирается в редакторе Visual Studio HTML.</span><span class="sxs-lookup"><span data-stu-id="24927-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="24927-234">![HTML-элемента, выделенного в редакторе Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "HTML-элемента, выделенного в редакторе Visual Studio HTML")</span><span class="sxs-lookup"><span data-stu-id="24927-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="24927-235">*HTML-элемента, выделенного в редакторе Visual Studio HTML*</span><span class="sxs-lookup"><span data-stu-id="24927-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="24927-236">Теперь мы обновим **front** класса CSS, чтобы изменить стиль выбранного элемента.</span><span class="sxs-lookup"><span data-stu-id="24927-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="24927-237">Чтобы сделать это, нажмите клавишу **CTRL** + **,** открыть **перейти к** поле поиска.</span><span class="sxs-lookup"><span data-stu-id="24927-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="24927-238">Тип **site.css** и нажмите клавишу **ввод** для открытия файла.</span><span class="sxs-lookup"><span data-stu-id="24927-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="24927-239">![Открытие файла Site.css](visual-studio-2013-web-tools/_static/image16.png "при открытии файла Site.css")</span><span class="sxs-lookup"><span data-stu-id="24927-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="24927-240">*При открытии файла Site.css*</span><span class="sxs-lookup"><span data-stu-id="24927-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="24927-241">Нажмите клавишу **CTRL** + **F** и тип **.front .flip containter** найти селектора CSS.</span><span class="sxs-lookup"><span data-stu-id="24927-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="24927-242">Щелкните светло синий квадрат в свойстве границы класса, чтобы открыть палитру цветов.</span><span class="sxs-lookup"><span data-stu-id="24927-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="24927-243">![Открытие палитры цветов](visual-studio-2013-web-tools/_static/image17.png "Открытие палитры цветов")</span><span class="sxs-lookup"><span data-stu-id="24927-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="24927-244">*Открытие палитры цветов*</span><span class="sxs-lookup"><span data-stu-id="24927-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="24927-245">Разверните палитра цветов, нажав кнопку шеврона и выберите новый цвет.</span><span class="sxs-lookup"><span data-stu-id="24927-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="24927-246">![Развернув палитра](visual-studio-2013-web-tools/_static/image18.png "расширение палитра цветов")</span><span class="sxs-lookup"><span data-stu-id="24927-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="24927-247">*Развернув палитра цветов*</span><span class="sxs-lookup"><span data-stu-id="24927-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="24927-248">Нажмите клавишу **CTRL** + **ALT** + **ввод** обновлять связанные браузеры.</span><span class="sxs-lookup"><span data-stu-id="24927-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="24927-249">Перейдите в Internet Explorer и обратите внимание на то, как изменился цвет границы.</span><span class="sxs-lookup"><span data-stu-id="24927-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="24927-250">![Internet Explorer — цвет границы](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer — цвет границы")</span><span class="sxs-lookup"><span data-stu-id="24927-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="24927-251">*Internet Explorer — цвет границы*</span><span class="sxs-lookup"><span data-stu-id="24927-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="24927-252">Перейдите в Google Chrome и обратите внимание на то, как изменился цвет границы.</span><span class="sxs-lookup"><span data-stu-id="24927-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="24927-253">![Google Chrome: цвет границы](visual-studio-2013-web-tools/_static/image20.png "Google Chrome: цвет границы")</span><span class="sxs-lookup"><span data-stu-id="24927-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="24927-254">*Google Chrome: цвет границы*</span><span class="sxs-lookup"><span data-stu-id="24927-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="24927-255">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="24927-256">Перейдите в конец **Site.css** файл и нажмите кнопку **CTRL** + **F** искомого **.btn** селектор.</span><span class="sxs-lookup"><span data-stu-id="24927-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="24927-257">Обратите внимание, что **- webkit-border-radius** свойство подчеркивается зеленым цветом.</span><span class="sxs-lookup"><span data-stu-id="24927-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="24927-258">![Свойство - webkit-border-radius селектора btn](visual-studio-2013-web-tools/_static/image21.png "свойство селекторе btn - webkit-border-radius")</span><span class="sxs-lookup"><span data-stu-id="24927-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="24927-259">*Свойство селекторе btn - webkit-border-radius*</span><span class="sxs-lookup"><span data-stu-id="24927-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="24927-260">Поместите курсор в **- webkit-border-radius** свойство.</span><span class="sxs-lookup"><span data-stu-id="24927-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="24927-261">Синяя линия должна появиться первую букву слова первого свойства.</span><span class="sxs-lookup"><span data-stu-id="24927-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="24927-262">Это **смарт-теге**.</span><span class="sxs-lookup"><span data-stu-id="24927-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="24927-263">Нажмите клавишу **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="24927-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="24927-264">Чтобы открыть меню предложений и нажмите кнопку **добавить отсутствующие стандартное свойство (радиус границы)**.</span><span class="sxs-lookup"><span data-stu-id="24927-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="24927-265">![Добавить отсутствует предложение стандартное свойство](visual-studio-2013-web-tools/_static/image22.png "добавить отсутствует предложение стандартное свойство")</span><span class="sxs-lookup"><span data-stu-id="24927-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="24927-266">*Добавление недостающих стандартное свойство предложение*</span><span class="sxs-lookup"><span data-stu-id="24927-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="24927-267">**Радиус границы** свойство автоматически добавляется к правилу CSS.</span><span class="sxs-lookup"><span data-stu-id="24927-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="24927-268">![Отсутствует стандартное свойство добавлены](visual-studio-2013-web-tools/_static/image23.png "отсутствует стандартное свойство добавлен")</span><span class="sxs-lookup"><span data-stu-id="24927-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="24927-269">*Отсутствует Добавление стандартные свойства*</span><span class="sxs-lookup"><span data-stu-id="24927-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="24927-270">Наведите указатель на **радиус границы** свойство для отображения **обозревателя матриц подсказки**.</span><span class="sxs-lookup"><span data-stu-id="24927-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="24927-271">**Обозревателя матриц подсказки** показывает доступность свойства в каждом браузере.</span><span class="sxs-lookup"><span data-stu-id="24927-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="24927-272">![Подсказка матрицы браузера](visual-studio-2013-web-tools/_static/image24.png "обозревателя матриц подсказки")</span><span class="sxs-lookup"><span data-stu-id="24927-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="24927-273">*Подсказка матрицы браузера*</span><span class="sxs-lookup"><span data-stu-id="24927-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="24927-274">Обратите внимание, что значение **радиус границы** свойство по-прежнему подчеркнутым.</span><span class="sxs-lookup"><span data-stu-id="24927-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="24927-275">Наведите указатель на значение, чтобы показать предупреждающее сообщение.</span><span class="sxs-lookup"><span data-stu-id="24927-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="24927-276">![Предупреждение значение свойства border-radius](visual-studio-2013-web-tools/_static/image25.png "предупреждение значение свойства Border-radius")</span><span class="sxs-lookup"><span data-stu-id="24927-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="24927-277">*Предупреждение значение свойства border-radius*</span><span class="sxs-lookup"><span data-stu-id="24927-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="24927-278">Удалите устройство из **радиус границы** значение свойства, как указано во всплывающей подсказке.</span><span class="sxs-lookup"><span data-stu-id="24927-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="24927-279">Как **радиус границы** — это стандартное свойство для определения границу как со скругленными углами, являются углов, можно удалить **- webkit-border-radius** свойство и значение из правила CSS.</span><span class="sxs-lookup"><span data-stu-id="24927-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="24927-280">Поместите курсор в **словам** свойство и обратите внимание, что смарт-тег, который также отображается ниже.</span><span class="sxs-lookup"><span data-stu-id="24927-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="24927-281">Откройте меню и щелкните **добавьте отсутствующие особенности поставщика**.</span><span class="sxs-lookup"><span data-stu-id="24927-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="24927-282">![Добавление отсутствующих предложение поставщика особенности](visual-studio-2013-web-tools/_static/image26.png "добавьте отсутствует предложение особенности поставщика")</span><span class="sxs-lookup"><span data-stu-id="24927-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="24927-283">*Добавление отсутствующих предложение особенности поставщика*</span><span class="sxs-lookup"><span data-stu-id="24927-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="24927-284">**-Ms-словам** свойство автоматически добавляется к правилу CSS.</span><span class="sxs-lookup"><span data-stu-id="24927-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="24927-285">![Свойство конкретного поставщика, добавленное](visual-studio-2013-web-tools/_static/image27.png "свойство конкретного поставщика, добавленное")</span><span class="sxs-lookup"><span data-stu-id="24927-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="24927-286">*Свойство конкретного поставщика, добавленное*</span><span class="sxs-lookup"><span data-stu-id="24927-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="24927-287">Задача 4 — обновление кода HTML из браузера</span><span class="sxs-lookup"><span data-stu-id="24927-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="24927-288">В этой задаче вы воспользуетесь связь с браузером **режим конструктора** возможность изменить объект DOM из браузера и передают эти изменения в исходный файл HTML в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24927-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="24927-289">В Google Chrome, нажмите клавишу **CTRL** + **ALT** + **D** для перехода в режим конструктора.</span><span class="sxs-lookup"><span data-stu-id="24927-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="24927-290">Наведите указатель на **Lorem Ipsum dolor sit amet** метки и нажмите кнопку.</span><span class="sxs-lookup"><span data-stu-id="24927-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="24927-291">![Редактирование вопрос](visual-studio-2013-web-tools/_static/image28.png "редактирования вопрос")</span><span class="sxs-lookup"><span data-stu-id="24927-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="24927-292">*Изменение вопроса*</span><span class="sxs-lookup"><span data-stu-id="24927-292">*Editing the question*</span></span>
3. <span data-ttu-id="24927-293">Должна появиться курсора.</span><span class="sxs-lookup"><span data-stu-id="24927-293">A cursor should appear.</span></span> <span data-ttu-id="24927-294">Заменить исходный с *как он выглядит когда я пишу больше вопрос?*, а затем нажмите клавишу **ESC** чтобы выйти из режима конструктора.</span><span class="sxs-lookup"><span data-stu-id="24927-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="24927-295">![Изменить вопрос](visual-studio-2013-web-tools/_static/image29.png "изменить вопрос")</span><span class="sxs-lookup"><span data-stu-id="24927-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="24927-296">*Изменить вопрос*</span><span class="sxs-lookup"><span data-stu-id="24927-296">*Question edited*</span></span>
4. <span data-ttu-id="24927-297">Коммутатор, вернитесь в Visual Studio и откройте **Index.cshtml**, если еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="24927-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="24927-298">Обратите внимание, что текст внутри **&lt;p&gt;** был обновлен элемент.</span><span class="sxs-lookup"><span data-stu-id="24927-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="24927-299">![Вопрос Updated в HTML-страницу](visual-studio-2013-web-tools/_static/image30.png "Updated вопрос в HTML-страницы")</span><span class="sxs-lookup"><span data-stu-id="24927-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="24927-300">*Обновленные вопрос в HTML-страницы*</span><span class="sxs-lookup"><span data-stu-id="24927-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="24927-301">Задача 5 - проверка SEO связанные предупреждения</span><span class="sxs-lookup"><span data-stu-id="24927-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="24927-302">**Оптимизация для поисковых** (систем SEO) — это процесс создания высокий ранг веб-сайта на механизм поиска, список результатов.</span><span class="sxs-lookup"><span data-stu-id="24927-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="24927-303">Чем выше ранжирует сайта и неизменно разрабатывать более качественное его в списке, дополнительные посетители сайта будут получены из этой поисковой системы.</span><span class="sxs-lookup"><span data-stu-id="24927-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="24927-304">Web Essentials включает в себя аналитического инструмента, который проверяет HTML, отчеты проблемы найден и предоставляет помощь для их исправления.</span><span class="sxs-lookup"><span data-stu-id="24927-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="24927-305">Перейдите к **представление** меню и выберите пункт **список ошибок** открыть **список ошибок** окна.</span><span class="sxs-lookup"><span data-stu-id="24927-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="24927-306">![Меню в представлении списка ошибок](visual-studio-2013-web-tools/_static/image31.png "список ошибок, в меню \"Вид\"")</span><span class="sxs-lookup"><span data-stu-id="24927-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="24927-307">*Меню в представлении списка ошибок*</span><span class="sxs-lookup"><span data-stu-id="24927-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="24927-308">Обратите внимание, что предупреждение об оптимизации поисковой системы, уведомлением о том, что **&lt;meta&gt;** тегов для описания страницы отсутствует.</span><span class="sxs-lookup"><span data-stu-id="24927-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="24927-309">Дважды щелкните запись предупреждения SEO ее устранению.</span><span class="sxs-lookup"><span data-stu-id="24927-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="24927-310">![Окно списка ошибок](visual-studio-2013-web-tools/_static/image32.png "окно списка ошибок")</span><span class="sxs-lookup"><span data-stu-id="24927-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="24927-311">*Окно "Список ошибок"*</span><span class="sxs-lookup"><span data-stu-id="24927-311">*Error List window*</span></span>
3. <span data-ttu-id="24927-312">В **Web Essentials** диалоговом окне щелкните **Да** вставляемый описание &lt;meta&gt; тега.</span><span class="sxs-lookup"><span data-stu-id="24927-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="24927-313">![Диалоговое окно Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Web Essentials диалоговое окно")</span><span class="sxs-lookup"><span data-stu-id="24927-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="24927-314">*Диалоговое окно Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="24927-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="24927-315">Редактор для  **\_Layout.cshtml** открывает и **&lt;meta&gt;** автоматически добавляется тег **head** раздел HTML-файл.</span><span class="sxs-lookup"><span data-stu-id="24927-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="24927-316">![Мета-тег, автоматически добавляются на странице _Layout](visual-studio-2013-web-tools/_static/image34.png "мета-тег, автоматически добавляется в _Layout страницы")</span><span class="sxs-lookup"><span data-stu-id="24927-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="24927-317">*Мета-тег, автоматически добавляется в \_страницу макета*</span><span class="sxs-lookup"><span data-stu-id="24927-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="24927-318">Измените значение свойства **содержимого** атрибут *GeekQuiz* и сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="24927-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="24927-319">Упражнение 2: Использование преимуществ фрагменты кода и IntelliSense</span><span class="sxs-lookup"><span data-stu-id="24927-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="24927-320">С помощью Web Essentials редактор HTML был расширен объектом дополнительные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="24927-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="24927-321">В этом упражнении вы увидите некоторые новые возможности, которые могут быть полезны при разработке веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="24927-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="24927-322">Задача 1 - использование технологии IntelliSense в HTML-документы</span><span class="sxs-lookup"><span data-stu-id="24927-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="24927-323">Первая новая возможность, вы увидите в этой задаче называется **динамического IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="24927-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="24927-324">Динамические IntelliSense считывает другие теги и атрибуты для определения возможных идентификаторов, которые будут использоваться.</span><span class="sxs-lookup"><span data-stu-id="24927-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="24927-325">В этой задаче вы создадите новый элемент формы HTML, который содержит метки и текстового поля ввода данных.</span><span class="sxs-lookup"><span data-stu-id="24927-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="24927-326">Затем вы добавите **для** атрибута на метку, чтобы привязать его к входные данные, и вы увидите предложения IntelliSense на основе идентификаторов входных параметров в области видимости.</span><span class="sxs-lookup"><span data-stu-id="24927-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="24927-327">Откройте **Visual Studio Express 2013 для Web** и **Begin.sln** решение находится в **источника/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/начало** папки.</span><span class="sxs-lookup"><span data-stu-id="24927-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="24927-328">Кроме того можно продолжить с решением, полученный в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="24927-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="24927-329">В **обозревателе решений**откройте **Index.cshtml** файл, расположенный в **представления** | **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="24927-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="24927-330">Добавьте следующие формы внутри **&lt;разделе&gt;** элемент.</span><span class="sxs-lookup"><span data-stu-id="24927-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="24927-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *формы*)</span><span class="sxs-lookup"><span data-stu-id="24927-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="24927-332">Тег input должно предшествовать метка с некоторые описанием этого поля.</span><span class="sxs-lookup"><span data-stu-id="24927-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="24927-333">Добавьте следующую метку перед тега входных данных.</span><span class="sxs-lookup"><span data-stu-id="24927-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="24927-334">**Для** атрибут **&lt;метка&gt;** указывает, какой элемент формы a метка связан.</span><span class="sxs-lookup"><span data-stu-id="24927-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="24927-335">Значение атрибута должно быть равным идентификатор связанного элемента.</span><span class="sxs-lookup"><span data-stu-id="24927-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="24927-336">Добавить **для** атрибут **&lt;метка&gt;** элемент.</span><span class="sxs-lookup"><span data-stu-id="24927-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="24927-337">Как показано на следующем рисунке, &quot;имя&quot; значение появляется в диалоговом окне IntelliSense на основе идентификатора элементов в той же области (содержащего  **&lt;формы&gt;**).</span><span class="sxs-lookup"><span data-stu-id="24927-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="24927-338">![В IntelliSense отображаются идентификатор](visual-studio-2013-web-tools/_static/image35.png "отображается идентификатор в IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="24927-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="24927-339">*Отображение идентификатор в IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="24927-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="24927-340">Удалить недавно добавленных **&lt;формы&gt;** элемента и его содержимым.</span><span class="sxs-lookup"><span data-stu-id="24927-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="24927-341">Задача 2 — с помощью фрагментов кода HTML</span><span class="sxs-lookup"><span data-stu-id="24927-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="24927-342">HTML5 появилась более чем 25 новых семантические теги.</span><span class="sxs-lookup"><span data-stu-id="24927-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="24927-343">Visual Studio уже есть поддержка IntelliSense для этих тегов, но Visual Studio 2013 позволяет быстрее и проще создавать разметку путем добавления новых фрагментов кода.</span><span class="sxs-lookup"><span data-stu-id="24927-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="24927-344">Хотя эти теги не являются сложными, они поставляются с несколько небольших тонкостей, таких как добавление резервные варианты соответствующий кодек для *аудио* тега.</span><span class="sxs-lookup"><span data-stu-id="24927-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="24927-345">В этой задаче вы увидите фрагменты кода HTML для тег аудио.</span><span class="sxs-lookup"><span data-stu-id="24927-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="24927-346">В **Index.cshtml** введите  **&lt;aud** внутри **&lt;разделе&gt;** элемент, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="24927-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="24927-347">![Вставка аудио элемента](visual-studio-2013-web-tools/_static/image36.png "Вставка аудио элемента")</span><span class="sxs-lookup"><span data-stu-id="24927-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="24927-348">*Вставка аудио элемента*</span><span class="sxs-lookup"><span data-stu-id="24927-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="24927-349">Нажмите клавишу **ВКЛАДКЕ** дважды и обратите внимание на то, как добавляется следующий код на странице, и курсор помещается на **src** атрибут первого источника.</span><span class="sxs-lookup"><span data-stu-id="24927-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="24927-350">Нажав **ВКЛАДКЕ** ключа дважды, вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="24927-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="24927-351">Аудио фрагмент стандартного использования *аудио* тег, с помощью двух файлов исходного кода для Улучшенная поддержка.</span><span class="sxs-lookup"><span data-stu-id="24927-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="24927-352">Удалите вторую строку и обновления источника первой строки с следующую ссылку, чтобы показать WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="24927-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="24927-353">Ниже приведен результирующий код.</span><span class="sxs-lookup"><span data-stu-id="24927-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="24927-354">Исходный файл *KatanaProject.mp3* используется в качестве примера.</span><span class="sxs-lookup"><span data-stu-id="24927-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="24927-355">При желании можно использовать другой источник.</span><span class="sxs-lookup"><span data-stu-id="24927-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="24927-356">Нажмите клавишу **CTRL** + **S** для сохранения файла.</span><span class="sxs-lookup"><span data-stu-id="24927-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="24927-357">Нажмите клавишу **CTRL** + **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="24927-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="24927-358">Обратите внимание на то, что аудио-плейер был добавлен в приложение.</span><span class="sxs-lookup"><span data-stu-id="24927-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="24927-359">![Аудио-плейер в Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "аудио player в Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="24927-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="24927-360">*Аудио-плейер в Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="24927-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="24927-361">![Аудио-плейер в Google Chrome](visual-studio-2013-web-tools/_static/image38.png "проигрыватель аудио в Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="24927-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="24927-362">*Аудио-плейер в Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="24927-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="24927-363">Не закрывайте браузеров.</span><span class="sxs-lookup"><span data-stu-id="24927-363">Do not close the browsers.</span></span> <span data-ttu-id="24927-364">В следующей задаче будет использовать их.</span><span class="sxs-lookup"><span data-stu-id="24927-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="24927-365">Задача 3 - использование технологии IntelliSense в документах JavaScript</span><span class="sxs-lookup"><span data-stu-id="24927-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="24927-366">С помощью Web Essentials 2013 стилей и HTML-страницы создания списка идентификаторов и имен классов.</span><span class="sxs-lookup"><span data-stu-id="24927-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="24927-367">В этой задаче вы узнаете, как эти списки, улучшить поддержку IntelliSense для JavaScript в Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="24927-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="24927-368">В **Index.cshtml** добавьте следующий код, чтобы определить **скрипт** тег для кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24927-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="24927-369">Добавьте следующий код внутри **скрипт** тег, чтобы определить функцию обратного вызова все готово.</span><span class="sxs-lookup"><span data-stu-id="24927-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="24927-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="24927-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="24927-371">Поместите курсор в **сценарий** тег и нажмите клавишу **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="24927-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="24927-372">Чтобы открыть меню «предложение».</span><span class="sxs-lookup"><span data-stu-id="24927-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="24927-373">Нажмите кнопку **извлечь файл**.</span><span class="sxs-lookup"><span data-stu-id="24927-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="24927-374">![Извлеките файл предложение JavaScript](visual-studio-2013-web-tools/_static/image39.png "JavaScript извлечь файл предложение")</span><span class="sxs-lookup"><span data-stu-id="24927-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="24927-375">*Извлеките файл предложение JavaScript*</span><span class="sxs-lookup"><span data-stu-id="24927-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="24927-376">В **Сохранить как** выберите **сценарии** папку, имя файла **init.js** и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="24927-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="24927-377">![Сохранить как окно](visual-studio-2013-web-tools/_static/image40.png "окно Сохранить как")</span><span class="sxs-lookup"><span data-stu-id="24927-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="24927-378">*Окно Сохранить как*</span><span class="sxs-lookup"><span data-stu-id="24927-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-379">**Init.js** файл создается и перемещения содержимого сценария в файл.</span><span class="sxs-lookup"><span data-stu-id="24927-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="24927-380">![Init.js файл, созданный с помощью содержимого](visual-studio-2013-web-tools/_static/image41.png "Init.js файл, созданный с помощью содержимого пакета")</span><span class="sxs-lookup"><span data-stu-id="24927-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="24927-381">*Init.js файл, созданный с помощью содержимого пакета*</span><span class="sxs-lookup"><span data-stu-id="24927-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="24927-382">Откройте **Index.cshtml** файл и проверьте, что тег сценария было заменено значением ссылку **init.js** файла.</span><span class="sxs-lookup"><span data-stu-id="24927-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="24927-383">![Справочник по html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js ссылка html")</span><span class="sxs-lookup"><span data-stu-id="24927-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="24927-384">*Справочник по html init.js*</span><span class="sxs-lookup"><span data-stu-id="24927-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="24927-385">Перейдите к **обозревателе решений** и обратите внимание, что **init.js** файл был автоматически включен в решение.</span><span class="sxs-lookup"><span data-stu-id="24927-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="24927-386">![Файл init.js, включенных в решение](visual-studio-2013-web-tools/_static/image43.png "Init.js файл, включенный в решение")</span><span class="sxs-lookup"><span data-stu-id="24927-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="24927-387">*Файл init.js, включенных в решение*</span><span class="sxs-lookup"><span data-stu-id="24927-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="24927-388">Вернитесь в **init.js** файл, чтобы обновить **готовы** функцию обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="24927-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="24927-389">В определении функции обратного вызова, который передается *готовы*, добавьте следующий код, чтобы получить все элементы с помощью атрибутов определенного класса.</span><span class="sxs-lookup"><span data-stu-id="24927-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="24927-390">Нажмите клавишу **CTRL** + **пространства** в кавычках внутри **getElementsByClassName** вызов функции.</span><span class="sxs-lookup"><span data-stu-id="24927-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="24927-391">![Отображение IntelliSense для функции getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "отображения IntelliSense для функции getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="24927-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="24927-392">*Отображение IntelliSense для функции getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="24927-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-393">Обратите внимание, что IntelliSense показывает классы, определенные в таблицах стилей проекта.</span><span class="sxs-lookup"><span data-stu-id="24927-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="24927-394">Замените строку, созданный следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="24927-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="24927-395">Поместите курсор после **au** внутри кавычек в **getElementsByTagName** функцию и нажмите клавишу **CTRL** + **пространства**.</span><span class="sxs-lookup"><span data-stu-id="24927-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="24927-396">![Отображение IntelliSense для метода getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "отображения IntelliSense для метода getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="24927-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="24927-397">*Отображение IntelliSense для метод getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="24927-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="24927-398">Выберите **&quot;аудио&quot;** в списке и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="24927-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="24927-399">Результат показан на примере ниже.</span><span class="sxs-lookup"><span data-stu-id="24927-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="24927-400">![Получение элементов, аудио](visual-studio-2013-web-tools/_static/image46.png "извлечение аудио элементов")</span><span class="sxs-lookup"><span data-stu-id="24927-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="24927-401">*Получение элементов, аудио*</span><span class="sxs-lookup"><span data-stu-id="24927-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="24927-402">В **обозревателе решений**, щелкните правой кнопкой мыши **init.js** файл **сценарии** папку и выберите **JavaScript для уменьшения размера файлов** из **Web Essentials** меню.</span><span class="sxs-lookup"><span data-stu-id="24927-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="24927-403">![Уменьшить размер файлов JavaScript](visual-studio-2013-web-tools/_static/image47.png "JavaScript для уменьшения размера файлов")</span><span class="sxs-lookup"><span data-stu-id="24927-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="24927-404">*Уменьшить размер файлов JavaScript*</span><span class="sxs-lookup"><span data-stu-id="24927-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="24927-405">При появлении запроса на включение автоматического минификации, при изменении исходного файла, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="24927-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="24927-406">![Включение автоматического минификации предупреждение](visual-studio-2013-web-tools/_static/image48.png "Включение автоматического минификации предупреждение")</span><span class="sxs-lookup"><span data-stu-id="24927-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="24927-407">*Включение автоматического минификации предупреждение*</span><span class="sxs-lookup"><span data-stu-id="24927-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="24927-408">**Init.min.js** создается и добавляется в качестве зависимости **init.js** файла.</span><span class="sxs-lookup"><span data-stu-id="24927-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="24927-409">![Созданный файл init.min.js](visual-studio-2013-web-tools/_static/image49.png "Init.min.js файл, созданный")</span><span class="sxs-lookup"><span data-stu-id="24927-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="24927-410">*Созданный файл init.min.js*</span><span class="sxs-lookup"><span data-stu-id="24927-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="24927-411">Откройте **init.min.js** файл и обратите внимание на то, что файл будет уменьшено.</span><span class="sxs-lookup"><span data-stu-id="24927-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="24927-412">![Содержимое файла init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js содержимое файла")</span><span class="sxs-lookup"><span data-stu-id="24927-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="24927-413">*Содержимое файла init.min.js*</span><span class="sxs-lookup"><span data-stu-id="24927-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="24927-414">В **init.js** добавьте следующий код ниже **getElementsByTagName** вызов для воспроизведения аудио элементы функции.</span><span class="sxs-lookup"><span data-stu-id="24927-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="24927-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="24927-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="24927-416">Нажмите кнопку **CTRL** + **S** для сохранения файла.</span><span class="sxs-lookup"><span data-stu-id="24927-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="24927-417">Поскольку минифицированные файл уже открыт, появится диалоговое окно, о том, что файл был изменен вне редактора исходного кода.</span><span class="sxs-lookup"><span data-stu-id="24927-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="24927-418">Нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="24927-418">Click **Yes**.</span></span>

    <span data-ttu-id="24927-419">![Microsoft Visual Studio предупреждение](visual-studio-2013-web-tools/_static/image51.png "предупреждение Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="24927-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="24927-420">*Microsoft Visual Studio предупреждение*</span><span class="sxs-lookup"><span data-stu-id="24927-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="24927-421">Вернитесь в **init.min.js** файл, чтобы убедиться, что файл был обновлен с новым кодом.</span><span class="sxs-lookup"><span data-stu-id="24927-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="24927-422">![Файл init.min.js обновлен](visual-studio-2013-web-tools/_static/image52.png "Init.min.js файл обновлен")</span><span class="sxs-lookup"><span data-stu-id="24927-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="24927-423">*Init.min.js файл обновлен*</span><span class="sxs-lookup"><span data-stu-id="24927-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="24927-424">Нажмите кнопку **обновления в браузере ссылку** кнопки.</span><span class="sxs-lookup"><span data-stu-id="24927-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="24927-425">Когда обновляются оба браузера аудиоплеера, полученный в предыдущей задаче начнется автоматически.</span><span class="sxs-lookup"><span data-stu-id="24927-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="24927-426">![Аудиоплеер, включенные в представлении](visual-studio-2013-web-tools/_static/image53.png "проигрыватель аудио, включены в представлении")</span><span class="sxs-lookup"><span data-stu-id="24927-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="24927-427">*Аудиоплеер, включенные в представлении*</span><span class="sxs-lookup"><span data-stu-id="24927-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="24927-428">Сводка</span><span class="sxs-lookup"><span data-stu-id="24927-428">Summary</span></span>

<span data-ttu-id="24927-429">Выполнив этой практической работу вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="24927-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="24927-430">Используйте новые возможности редактора HTML, включенные в Web Essentials, например форматированного фрагменты кода HTML5 и Zen кодирования</span><span class="sxs-lookup"><span data-stu-id="24927-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="24927-431">Используйте новые возможности редактора CSS, включенные в Web Essentials, такие как палитра цветов и обозревателя матриц подсказки</span><span class="sxs-lookup"><span data-stu-id="24927-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="24927-432">Используйте новые возможности редактора JavaScript, включенные в Web Essentials, например извлечь файл, а также IntelliSense для всех элементов HTML</span><span class="sxs-lookup"><span data-stu-id="24927-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="24927-433">Обмен данными между обозревателем и Visual Studio с помощью привязывания к браузеру</span><span class="sxs-lookup"><span data-stu-id="24927-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
