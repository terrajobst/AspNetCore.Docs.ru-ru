---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Новые возможности ASP.NET и веб-разработки в Visual Studio 2012 | Документы Microsoft
author: rick-anderson
description: Новую версию Visual Studio появился ряд улучшений, ориентированы на улучшение качества и производительности при работе с веб-технологиями...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 00b43cc548df44edded925521991a095ed856494
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="32b9e-103">Новые возможности ASP.NET и веб-разработки в Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="32b9e-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="32b9e-104">По [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="32b9e-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="32b9e-105">Новую версию Visual Studio появился ряд улучшений, ориентированы на улучшение качества и производительности при работе с веб-технологий.</span><span class="sxs-lookup"><span data-stu-id="32b9e-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="32b9e-106">Редакторы Visual Studio для CSS, JavaScript и HTML полностью переработан для включения многие из средств кода наиболее запросу, такие как IntelliSense и автоматического отступа.</span><span class="sxs-lookup"><span data-stu-id="32b9e-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="32b9e-107">О производительности объединение и Минификация теперь интегрированы как встроенные функции, чтобы легко уменьшить страницы время загрузки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="32b9e-108">Visual Studio позволяет работать с новейшие технологии веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="32b9e-109">Межплатформенный CSS3 фрагменты можно использовать, чтобы убедиться в том, что веб-узла работает независимо от платформы клиента с использованием также преимуществ новых возможностей и HTML5 элементов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="32b9e-110">Написание и профилирование кода JavaScript должно быть проще с данной версией Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="32b9e-111">Списки IntelliSense интеграции XML документации и функции навигации теперь доступны для кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="32b9e-112">Теперь у вас есть каталог JavaScript под рукой.</span><span class="sxs-lookup"><span data-stu-id="32b9e-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="32b9e-113">Кроме того можно проверить соответствие ECMAScript5 со скриптами и обнаружить синтаксические ошибки на ранней стадии.</span><span class="sxs-lookup"><span data-stu-id="32b9e-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="32b9e-114">Последнее, но не менее эта версия Visual Studio реализует встроенных объединение и Минификация.</span><span class="sxs-lookup"><span data-stu-id="32b9e-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="32b9e-115">Файлы скриптов и таблиц стилей будет упаковываются и сжимаются, чтобы сайт выполняет быстрее.</span><span class="sxs-lookup"><span data-stu-id="32b9e-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="32b9e-116">Эта лаборатория рассматриваются улучшения и новые функции, описанные ранее путем применения незначительные изменения примера веб-приложения в папке исходных файлов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="32b9e-117">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="32b9e-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="32b9e-118">Цели</span><span class="sxs-lookup"><span data-stu-id="32b9e-118">Objectives</span></span>

<span data-ttu-id="32b9e-119">В этом Практическое лабораторное занятие, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="32b9e-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="32b9e-120">Использование новых возможностей и усовершенствований в редакторе CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="32b9e-121">Использование новых возможностей и усовершенствований в редакторе HTML</span><span class="sxs-lookup"><span data-stu-id="32b9e-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="32b9e-122">Использование новых возможностей и усовершенствований в редакторе JavaScript</span><span class="sxs-lookup"><span data-stu-id="32b9e-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="32b9e-123">Настройка и использование объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="32b9e-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="32b9e-124">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="32b9e-124">Prerequisites</span></span>

- <span data-ttu-id="32b9e-125">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="32b9e-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="32b9e-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (для сценариев установки - уже установлен на Windows 8 и Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="32b9e-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="32b9e-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - или в браузере совместимые HTML5</span><span class="sxs-lookup"><span data-stu-id="32b9e-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="32b9e-128">Упражнения</span><span class="sxs-lookup"><span data-stu-id="32b9e-128">Exercises</span></span>

<span data-ttu-id="32b9e-129">Это Практическое лабораторное занятие содержит следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="32b9e-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="32b9e-130">Упражнение 1: Новые возможности в редакторе CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="32b9e-131">Упражнение 2: Новые возможности в редакторе HTML</span><span class="sxs-lookup"><span data-stu-id="32b9e-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="32b9e-132">Упражнение 3: Новые возможности редактора JavaScript</span><span class="sxs-lookup"><span data-stu-id="32b9e-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="32b9e-133">Упражнение 4: Объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="32b9e-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="32b9e-134">Предполагаемое время для выполнения этого занятия: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="32b9e-135">Упражнение 1: Новые возможности в редакторе CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="32b9e-136">Веб-разработчики должны быть знакомы со многими трудностей, связанных с редактирования CSS.</span><span class="sxs-lookup"><span data-stu-id="32b9e-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="32b9e-137">Одним из самых серьезных проблем Дизайн CSS является различными браузерами.</span><span class="sxs-lookup"><span data-stu-id="32b9e-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="32b9e-138">Часто бывает, что после применения стилей на сайте, вы Обратите внимание, она выглядит иначе при открытии в другой браузер или устройство.</span><span class="sxs-lookup"><span data-stu-id="32b9e-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="32b9e-139">Таким образом может тратят значительное время на решении этих проблем visual поймет, что когда наконец работы в одном браузере, она разбивается в других.</span><span class="sxs-lookup"><span data-stu-id="32b9e-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="32b9e-140">Visual Studio теперь включает функции, помогающие разработчикам работать и доступа к эффективно Упорядочить таблицы стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="32b9e-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="32b9e-141">В рамках этого упражнения будет соответствовать новые возможности для эффективной организации и выпуска, а также CSS3 фрагментов кода на предмет совместимости браузерах.</span><span class="sxs-lookup"><span data-stu-id="32b9e-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="32b9e-142">Задача 1 - новые возможности редактора</span><span class="sxs-lookup"><span data-stu-id="32b9e-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="32b9e-143">В этой задаче будет обнаруживать новые возможности редактора CSS.</span><span class="sxs-lookup"><span data-stu-id="32b9e-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="32b9e-144">Этот новый редактор поможет повысить продуктивность работы, используя преимущества новой смарт-отступ комментарии к коду Улучшенная и улучшенные список IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="32b9e-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="32b9e-145">Запуск **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="32b9e-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="32b9e-146">В обозревателе решений откройте **Site.css** файла, расположенного в **стили** папки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="32b9e-147">Убедитесь, что **текстовый редактор** средства видны на панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="32b9e-148">Чтобы сделать это, выберите **представление** | **панели инструментов** пункт меню, а также проверьте **текстовый редактор** параметры.</span><span class="sxs-lookup"><span data-stu-id="32b9e-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="32b9e-149">Можно будет заметить, что, начиная с этой новой версии, **комментарий** кнопки (![кнопку Комментарий](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) и **Uncomment** кнопки (![раскомментируйте кнопка](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) также включены для редактора CSS.</span><span class="sxs-lookup"><span data-stu-id="32b9e-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="32b9e-150">![Включение редактор и CSS-средств](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Включение редактора и инструментов CSS")</span><span class="sxs-lookup"><span data-stu-id="32b9e-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="32b9e-151">*Включение редактора и инструментов CSS*</span><span class="sxs-lookup"><span data-stu-id="32b9e-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="32b9e-152">Прокрутите кода и выберите любого определения класса CSS.</span><span class="sxs-lookup"><span data-stu-id="32b9e-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="32b9e-153">Нажмите кнопку **комментарий** (![кнопку Комментарий](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) кнопку, чтобы комментирование выбранных строк.</span><span class="sxs-lookup"><span data-stu-id="32b9e-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="32b9e-154">Нажмите кнопку **Uncomment** (![раскомментируйте кнопка](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) кнопку, чтобы отменить изменения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="32b9e-155">Нажмите кнопку **свернуть** (![свернуть](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) и **развернуть** (![разверните](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) кнопок, расположенных на левом краю текста.</span><span class="sxs-lookup"><span data-stu-id="32b9e-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="32b9e-156">Обратите внимание, что теперь можно скрыть стили, которые не используется, чтобы получить более четкое представление.</span><span class="sxs-lookup"><span data-stu-id="32b9e-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="32b9e-157">![Свертывание классов CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "классы CSS, сворачивание")</span><span class="sxs-lookup"><span data-stu-id="32b9e-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="32b9e-158">*Свертывание классов CSS*</span><span class="sxs-lookup"><span data-stu-id="32b9e-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="32b9e-159">Убедитесь, что смарт-отступ параметр.</span><span class="sxs-lookup"><span data-stu-id="32b9e-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="32b9e-160">Выберите **средства** | **параметры** пункт меню, а затем выберите **текстовый редактор** | **CSS**  |  **Форматирование** страницы в левой части экрана.</span><span class="sxs-lookup"><span data-stu-id="32b9e-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="32b9e-161">Проверьте **отступы** параметр.</span><span class="sxs-lookup"><span data-stu-id="32b9e-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="32b9e-162">![Включение отступы](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Включение отступы")</span><span class="sxs-lookup"><span data-stu-id="32b9e-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="32b9e-163">*Включение отступы*</span><span class="sxs-lookup"><span data-stu-id="32b9e-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="32b9e-164">Найдите определение основного класса (.main) и добавление элементов div стиля.</span><span class="sxs-lookup"><span data-stu-id="32b9e-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="32b9e-165">Обратите внимание, код автоматически, выравнивает помочь пользователям искать родительские классы краткий обзор.</span><span class="sxs-lookup"><span data-stu-id="32b9e-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="32b9e-166">CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="32b9e-167">![Иерархические выравнивание в CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "иерархических выравнивание в CSS")</span><span class="sxs-lookup"><span data-stu-id="32b9e-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="32b9e-168">*Иерархические выравнивание в CSS*</span><span class="sxs-lookup"><span data-stu-id="32b9e-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="32b9e-169">Внутри **.main div** класса, найдите курсора в конце **границы: 0px;** и нажмите клавишу **ввод** для отображения списка IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="32b9e-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="32b9e-170">Начните вводить **верхней** и обратите внимание на то, как список будет отфильтрован по мере ввода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="32b9e-171">В списке будут отображены элементы, содержащие **верхней** в любой части слова (в предыдущих версиях Visual Studio, список будет отфильтрован элементами, *начать* вместе с выражением).</span><span class="sxs-lookup"><span data-stu-id="32b9e-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="32b9e-172">![Усовершенствования IntelliSense в CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "усовершенствования IntelliSense в CSS")</span><span class="sxs-lookup"><span data-stu-id="32b9e-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="32b9e-173">*Усовершенствования IntelliSense в CSS*</span><span class="sxs-lookup"><span data-stu-id="32b9e-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="32b9e-174">Задача 2 - палитра цветов</span><span class="sxs-lookup"><span data-stu-id="32b9e-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="32b9e-175">В этой задаче будет обнаруживать новые палитра цветов CSS интегрируется в IntelliSense в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="32b9e-176">В **Site.css,** найдите определение класса заголовка (.header) и поместите курсор рядом с **цвет фона** атрибут между &quot;:&quot; и &quot; # &quot; символов в соответствующей строке кода **.**</span><span class="sxs-lookup"><span data-stu-id="32b9e-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="32b9e-177">![Поиск курсор](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "расположение курсора")</span><span class="sxs-lookup"><span data-stu-id="32b9e-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="32b9e-178">*Поиск курсора*</span><span class="sxs-lookup"><span data-stu-id="32b9e-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="32b9e-179">Удалить **двоеточие** (:) и записывают ее еще раз, чтобы отобразить палитру цветов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="32b9e-180">Обратите внимание, что первый цвета, которые вы увидите наиболее частые цвета веб-узла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="32b9e-181">Если щелкнуть белый цвет, его HTML-код (#fff) заменяет текущий код цвета в таблице стилей.</span><span class="sxs-lookup"><span data-stu-id="32b9e-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="32b9e-182">![Палитра цветов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "палитра цветов")</span><span class="sxs-lookup"><span data-stu-id="32b9e-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="32b9e-183">*Палитра цветов*</span><span class="sxs-lookup"><span data-stu-id="32b9e-183">*Color picker*</span></span>
3. <span data-ttu-id="32b9e-184">Нажмите клавишу **развернуть** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) кнопки в окне выбора цвета для отображения цветового градиента, а затем перетащите курсор, выбрать другой цвет градиента.</span><span class="sxs-lookup"><span data-stu-id="32b9e-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="32b9e-185">После этого щелкните **Пипетка** кнопку и выберите любой цвет на экране.</span><span class="sxs-lookup"><span data-stu-id="32b9e-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="32b9e-186">Обратите внимание, что значение цвета фона динамически изменяется при перемещении указателя мыши.</span><span class="sxs-lookup"><span data-stu-id="32b9e-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="32b9e-187">![Средство выбора градиента](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "средство выбора градиента")</span><span class="sxs-lookup"><span data-stu-id="32b9e-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="32b9e-188">*Средство выбора градиента*</span><span class="sxs-lookup"><span data-stu-id="32b9e-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="32b9e-189">В **непрозрачности** ползунок, переместить область выделения в центр окна, чтобы уменьшить степень непрозрачности.</span><span class="sxs-lookup"><span data-stu-id="32b9e-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="32b9e-190">Обратите внимание, что цвет фона значение изменится его масштаб RGBA.</span><span class="sxs-lookup"><span data-stu-id="32b9e-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="32b9e-191">![Палитра цветов непрозрачности](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "палитра непрозрачности")</span><span class="sxs-lookup"><span data-stu-id="32b9e-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="32b9e-192">*Выбор цвета прозрачности*</span><span class="sxs-lookup"><span data-stu-id="32b9e-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-193">Определение цветов RGBA (красный, зеленый, синий, альфа) в CSS3 позволяет определить значение непрозрачности цвета для одного элемента.</span><span class="sxs-lookup"><span data-stu-id="32b9e-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="32b9e-194">В отличие от **непрозрачности -** аналогичный атрибут CSS **-** цвета RGBA совместимы с новейших браузерах.</span><span class="sxs-lookup"><span data-stu-id="32b9e-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="32b9e-195">Задача 3 - совместимый фрагменты CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="32b9e-196">В этой задаче вы узнаете, как использовать совместимый CSS3 фрагменты межплатформенный для реализации некоторых возможностей веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="32b9e-197">В **Site.css** файла, найдите **заголовок** (.header) определения класса CSS и поместите курсор ниже **/ \*границы radius\* /** рамку, чтобы добавить новый фрагмент.</span><span class="sxs-lookup"><span data-stu-id="32b9e-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="32b9e-198">Нажмите клавишу **ввод** для отображения списка IntelliSense и тип **radius** для фильтрации списка.</span><span class="sxs-lookup"><span data-stu-id="32b9e-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="32b9e-199">Выберите **границы radius** параметр из списка с двойной щелчок и нажмите клавишу **ВКЛАДКЕ** ключа для вставки фрагмента.</span><span class="sxs-lookup"><span data-stu-id="32b9e-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="32b9e-200">Введите размер radius в пикселях и нажать **ввод**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="32b9e-201">Например, введите **15px**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="32b9e-202">Границы прямоугольника с закругленными в большинстве браузеров соответствия HTML5, включая Mozilla и основан на WebKit обозреватели подготавливается к просмотру CSS3 атрибуты, добавляемые в фрагменте.</span><span class="sxs-lookup"><span data-stu-id="32b9e-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="32b9e-203">![С помощью фрагмента radius границы](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "фрагмента границы radius")</span><span class="sxs-lookup"><span data-stu-id="32b9e-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="32b9e-204">*С помощью фрагмента границы radius*</span><span class="sxs-lookup"><span data-stu-id="32b9e-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="32b9e-205">Применить те же **границы** фрагменты кода в стиле страницы (.page).</span><span class="sxs-lookup"><span data-stu-id="32b9e-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="32b9e-206">CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="32b9e-207">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="32b9e-208">Обратите внимание, что каждая страница теперь скругленные границы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="32b9e-209">![Скругленные углы](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "скругленные углы")</span><span class="sxs-lookup"><span data-stu-id="32b9e-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="32b9e-210">*Закругленные углы*</span><span class="sxs-lookup"><span data-stu-id="32b9e-210">*Rounded corners*</span></span>
4. <span data-ttu-id="32b9e-211">Закройте окно браузера и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="32b9e-212">Откройте **Custom.css** файла, расположенного в **стили** папку и переместите курсор на **div.images ul li img** определения класса.</span><span class="sxs-lookup"><span data-stu-id="32b9e-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="32b9e-213">Нажмите клавишу ВВОД, чтобы отобразить список IntelliSense, тип **тень** и нажмите клавишу **ВКЛАДКЕ** ключ дважды, чтобы вставить фрагмент кода по умолчанию тень внутри определения класса.</span><span class="sxs-lookup"><span data-stu-id="32b9e-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="32b9e-214">Задайте значения тени **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="32b9e-215">Введите **границы radius** и вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="32b9e-216">Тип **15px** задать размер radius и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="32b9e-217">![Скругленные углы с тенью](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "скругленные углы с тенью")</span><span class="sxs-lookup"><span data-stu-id="32b9e-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="32b9e-218">*Закругленные углы с тенью*</span><span class="sxs-lookup"><span data-stu-id="32b9e-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-219">В настоящее время атрибут тени вставляется соответствующий префикс (moz, webkit, o), для поддержки Mozilla и браузеры Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="32b9e-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="32b9e-220">Создайте новый класс **div.images ul li img:hover** ниже **div.images ul li img** определения класса, поместите курсор в квадратных скобках **.**</span><span class="sxs-lookup"><span data-stu-id="32b9e-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="32b9e-221">CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="32b9e-222">Тип **преобразования** и нажмите клавишу **ВКЛАДКЕ** ключ дважды, чтобы вставить фрагмент преобразования.</span><span class="sxs-lookup"><span data-stu-id="32b9e-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="32b9e-223">Затем введите **rotate(-15deg)** Чтобы изменить значение угла поворота при прохождении в курсора или изображения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="32b9e-224">CSS</span><span class="sxs-lookup"><span data-stu-id="32b9e-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="32b9e-225">Нажмите клавишу **F5** для запуска решения и перейти на страницу CSS3.</span><span class="sxs-lookup"><span data-stu-id="32b9e-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="32b9e-226">Обратите внимание, что образы имеют скругленные углы поле тени.</span><span class="sxs-lookup"><span data-stu-id="32b9e-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="32b9e-227">Наведите указатель мыши на изображения и просматривать их поворот.</span><span class="sxs-lookup"><span data-stu-id="32b9e-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="32b9e-228">![Преобразование фрагмента Поворот изображения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "фрагмент преобразование, поворот изображения")</span><span class="sxs-lookup"><span data-stu-id="32b9e-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="32b9e-229">*Преобразование фрагмента Поворот изображения*</span><span class="sxs-lookup"><span data-stu-id="32b9e-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-230">Если используется Internet Explorer 10 и не могут видеть тени, убедитесь, что документ включен режим стандарты IE10.</span><span class="sxs-lookup"><span data-stu-id="32b9e-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="32b9e-231">Нажмите клавишу **F12** открытие средств разработчика Internet Explorer и нажмите кнопку **режим документов** изменение на стандарты IE10.</span><span class="sxs-lookup"><span data-stu-id="32b9e-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![о-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="32b9e-233">Упражнение 2: Новые возможности в редакторе HTML</span><span class="sxs-lookup"><span data-stu-id="32b9e-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="32b9e-234">Visual Studio использует улучшенные HTML-редактора.</span><span class="sxs-lookup"><span data-stu-id="32b9e-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="32b9e-235">Некоторые из улучшений, включенных в эту версию, интеллектуальных отступов в HTML-документы, фрагменты кода HTML5, HTML начала и окончания тег сопоставления и проверку HTML.</span><span class="sxs-lookup"><span data-stu-id="32b9e-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="32b9e-236">В рамках этого упражнения вы увидите, как эти изменения повышает степень вашей при работе в разметке веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="32b9e-237">Как редактор CSS HTML-редактор также улучшены.</span><span class="sxs-lookup"><span data-stu-id="32b9e-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="32b9e-238">Большинство этих улучшений являются небольшой, упрощения работы веб-разработчиков.</span><span class="sxs-lookup"><span data-stu-id="32b9e-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="32b9e-239">Действия, например дополнительные фрагменты кода для HTML5, интеллектуальных отступов сопоставления открывающий и закрывающий теги, когда изменения и проверки, предназначенных для HTML-документ DOCTYPE некоторые из этих усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="32b9e-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="32b9e-240">Задача 1 - Улучшенная DOCTYPE проверки</span><span class="sxs-lookup"><span data-stu-id="32b9e-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="32b9e-241">Редактор HTML теперь имеет возможность проверять DOCTYPE страницы, даже если определение может быть на главной странице.</span><span class="sxs-lookup"><span data-stu-id="32b9e-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="32b9e-242">В зависимости от DOCTYPE страницы редактор HTML будет проверять с нужного набора правил и будет фильтровать список IntelliSense, учитывая элементы DOCTYPE.</span><span class="sxs-lookup"><span data-stu-id="32b9e-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="32b9e-243">В этой задаче будут изменены DOCTYPE страницы, чтобы увидеть, как поведение редактора HTML изменяется соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="32b9e-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="32b9e-244">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="32b9e-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="32b9e-245">Откройте **Site.Master** страницы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="32b9e-246">Обратите внимание в целевой схеме для проверки панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="32b9e-247">Способ работает редактор HTML (проверки, IntelliSense, т. д.) правильно изменится в соответствии с выбранной Doctype.</span><span class="sxs-lookup"><span data-stu-id="32b9e-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="32b9e-248">![Использовать тип документа на редактирование HTML-кода инструментов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype использования инструментов редактирование HTML-кода")</span><span class="sxs-lookup"><span data-stu-id="32b9e-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="32b9e-249">*Использовать тип документа в панели инструментов редактирование HTML-кода*</span><span class="sxs-lookup"><span data-stu-id="32b9e-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="32b9e-250">Измените целевую схему для HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="32b9e-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="32b9e-251">![Изменение Doctype на редактирование HTML-кода инструментов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "изменение Doctype инструментов редактирование HTML-кода")</span><span class="sxs-lookup"><span data-stu-id="32b9e-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="32b9e-252">*Изменение описания типа документа в панели инструментов редактирование HTML-кода*</span><span class="sxs-lookup"><span data-stu-id="32b9e-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="32b9e-253">Поместите курсор в разделе **текст** элемент и начать ввод имени HTML5 элемента (например, **видео**).</span><span class="sxs-lookup"><span data-stu-id="32b9e-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="32b9e-254">Обратите внимание, что элемент не доступен в списке IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="32b9e-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="32b9e-255">![Не перечисленные элементы HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "отсутствует в списке элементов HTML5")</span><span class="sxs-lookup"><span data-stu-id="32b9e-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="32b9e-256">*Не перечисленные элементы HTML5*</span><span class="sxs-lookup"><span data-stu-id="32b9e-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="32b9e-257">Отменить изменения в целевую схему для проверки инструментов комплектации DOCTYPE: XHTML5 из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="32b9e-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="32b9e-258">![Использовать тип документа на редактирование HTML-кода инструментов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype использования инструментов редактирование HTML-кода")</span><span class="sxs-lookup"><span data-stu-id="32b9e-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="32b9e-259">*Сброс Doctype инструментов редактирование HTML-кода*</span><span class="sxs-lookup"><span data-stu-id="32b9e-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="32b9e-260">Поместите курсор в разделе **текст** элемент и начать ввод элемент HTML5 еще раз (например, такая как **видео**).</span><span class="sxs-lookup"><span data-stu-id="32b9e-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="32b9e-261">Обратите внимание, что элементы HTML5, теперь доступны в списке IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="32b9e-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="32b9e-262">![Элементы HTML5 списке](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "перечисленные элементы HTML5")</span><span class="sxs-lookup"><span data-stu-id="32b9e-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="32b9e-263">*Идет перечисленные элементы HTML5*</span><span class="sxs-lookup"><span data-stu-id="32b9e-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="32b9e-264">Задача 2 - начало/конец теги автоматического обновления</span><span class="sxs-lookup"><span data-stu-id="32b9e-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="32b9e-265">Теперь Visual Studio обновляет HTML-код для открытия или закрытия тегами элемента изменяемый соответствуют друг другу.</span><span class="sxs-lookup"><span data-stu-id="32b9e-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="32b9e-266">Эта новая функция улучшит производительность при редактировании HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="32b9e-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="32b9e-267">На **Default.aspx** добавьте **H3** элемента с заголовком (например, Visual Studio 2012 Rocks!).</span><span class="sxs-lookup"><span data-stu-id="32b9e-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
~~~
2. <span data-ttu-id="32b9e-268">Изменение **H3** тег и тип **H2** или **H1.**</span><span class="sxs-lookup"><span data-stu-id="32b9e-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="32b9e-269">Обратите внимание, что автоматически обновляет закрывающий тег.</span><span class="sxs-lookup"><span data-stu-id="32b9e-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="32b9e-270">Можно также изменить закрывающий тег, чтобы увидеть, что открывающий тег соответствующим образом обновляет слишком.</span><span class="sxs-lookup"><span data-stu-id="32b9e-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="32b9e-271">![Автоматическое обновление закрывающий тег](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "автоматическое обновление закрывающего тега")</span><span class="sxs-lookup"><span data-stu-id="32b9e-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="32b9e-272">*Автоматическое обновление закрывающего тега*</span><span class="sxs-lookup"><span data-stu-id="32b9e-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="32b9e-273">Задача 3 - новых фрагментов кода HTML5</span><span class="sxs-lookup"><span data-stu-id="32b9e-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="32b9e-274">Visual Studio теперь включает несколько фрагментов кода HTML5.</span><span class="sxs-lookup"><span data-stu-id="32b9e-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="32b9e-275">В этой задаче будет использовать некоторые из этих фрагментов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="32b9e-276">Добавить новую папку с именем **аудио** в корневую папку веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="32b9e-277">Откройте проводник Windows и скопируйте любые звуковой файл в **аудио** папки **WhatsNewASPNET.sln** решения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="32b9e-278">В **Default.aspx** найдите курсор под Web11 Rocks!!</span><span class="sxs-lookup"><span data-stu-id="32b9e-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="32b9e-279">Заголовок.</span><span class="sxs-lookup"><span data-stu-id="32b9e-279">Header.</span></span> <span data-ttu-id="32b9e-280">Тип **аудио** и нажмите клавишу TAB.</span><span class="sxs-lookup"><span data-stu-id="32b9e-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="32b9e-281">Новый редактор HTML фрагменты кода для содержимого HTML5.</span><span class="sxs-lookup"><span data-stu-id="32b9e-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="32b9e-282">Не забывайте использовать правильное определение DOCTYPE для включения фрагменты HTML5.</span><span class="sxs-lookup"><span data-stu-id="32b9e-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="32b9e-283">![Вставка фрагментов кода HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "вставки фрагментов HTML5")</span><span class="sxs-lookup"><span data-stu-id="32b9e-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="32b9e-284">*Вставка фрагментов кода HTML5*</span><span class="sxs-lookup"><span data-stu-id="32b9e-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="32b9e-285">Обновите источник звука, чтобы она указывала на существующий звуковой файл.</span><span class="sxs-lookup"><span data-stu-id="32b9e-285">Update the audio source to point to an existing audio file.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

> [!NOTE]
> You will need to add the audio file to the solution.
~~~
4. <span data-ttu-id="32b9e-286">Нажмите клавишу **F5** для запуска сайта и воспроизводить звук.</span><span class="sxs-lookup"><span data-stu-id="32b9e-286">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="32b9e-287">![Элемент управления звуком под управлением](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "под управлением элемент управления звуком")</span><span class="sxs-lookup"><span data-stu-id="32b9e-287">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="32b9e-288">*Элемент управления звуком под управлением*</span><span class="sxs-lookup"><span data-stu-id="32b9e-288">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-289">Можно также попытаться несколько фрагментов, которые включены в Visual Studio, такие как видео, на рисунке, и т. д.</span><span class="sxs-lookup"><span data-stu-id="32b9e-289">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="32b9e-290">Попробуйте вставить элемент управления в какую-то часть страницы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-290">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="32b9e-291">Например, при попытке вставить **GridView** управления, но вместо ввода  **&lt;линий сетки,** начните вводить  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-291">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="32b9e-292">Обратите внимание, что в списке IntelliSense отображаются **asp: GridView** элемента управления.</span><span class="sxs-lookup"><span data-stu-id="32b9e-292">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="32b9e-293">IntelliSense в редакторе HTML теперь предоставляет поиска регистр заголовков, а также частичного соответствия (получение всех элементов, содержащих термин).</span><span class="sxs-lookup"><span data-stu-id="32b9e-293">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="32b9e-294">![Вставка элемента управления GridView с использованием списков IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Вставка элемента управления GridView с использованием списков IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="32b9e-294">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="32b9e-295">*Вставка элемента управления GridView с использованием списков IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="32b9e-295">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="32b9e-296">Если ввести  **&lt;сетки** вы получите все элементы, которые соответствуют запросу, но Visual Studio предложит **gridview** управления:</span><span class="sxs-lookup"><span data-stu-id="32b9e-296">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="32b9e-297">![Вставка GridView со списками IntelliSense и частичного соответствия](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Вставка GridView со списками IntelliSense и частичного соответствия")</span><span class="sxs-lookup"><span data-stu-id="32b9e-297">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="32b9e-298">*Вставка GridView со списками IntelliSense и частичного соответствия*</span><span class="sxs-lookup"><span data-stu-id="32b9e-298">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="32b9e-299">Задача 4 - смарт-тегов HTML-редактора</span><span class="sxs-lookup"><span data-stu-id="32b9e-299">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="32b9e-300">Улучшение другой редактор HTML — смарт-теги.</span><span class="sxs-lookup"><span data-stu-id="32b9e-300">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="32b9e-301">Смарт-теги упрощают выполнение задач разработки обычных или повторяющихся отдельно для каждого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="32b9e-301">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="32b9e-302">Эта функция уже доступна в конструкторе HTML, но не в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="32b9e-302">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="32b9e-303">Откройте **Site.Master** и найдите **asp: меню** элемента.</span><span class="sxs-lookup"><span data-stu-id="32b9e-303">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="32b9e-304">Поместите курсор в открывающий тег и обратите внимание, что небольшой глифа, отображаемого в нижней части элемента - щелкните его, чтобы открыть меню смарт-тегов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-304">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="32b9e-305">Обратите внимание, что имеется быстрый доступ для некоторых задач, связанных с элементом управления меню.</span><span class="sxs-lookup"><span data-stu-id="32b9e-305">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="32b9e-306">![Задачи для элемента управления меню смарт-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "интеллектуальные задачи для элемента управления меню")</span><span class="sxs-lookup"><span data-stu-id="32b9e-306">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="32b9e-307">*Смарт-тегов для элемента управления меню*</span><span class="sxs-lookup"><span data-stu-id="32b9e-307">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="32b9e-308">Задача 5 - интеллектуальных отступов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-308">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="32b9e-309">Одно из рекомендации в формате HTML отступов вложенных элементов, чтобы обеспечить соответствие кода для чтения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-309">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="32b9e-310">В Visual Studio 2012 вы заметите, что редактор автоматически смещает элементы при написании кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-310">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="32b9e-311">В предыдущей версии Visual Studio, интеллектуальных отступов были доступны в редакторе XML, но не в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="32b9e-311">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="32b9e-312">Убедитесь, что конфигурация отступы в редакторе HTML присвоено интеллектуальных отступов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-312">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="32b9e-313">Чтобы сделать это, выберите **инструменты | Параметры** пункт меню, а затем выберите **текстовый редактор | HTML | Вкладки** страницы в левой части экрана.</span><span class="sxs-lookup"><span data-stu-id="32b9e-313">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="32b9e-314">Выберите параметр интеллектуальных отступов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-314">Select the Smart indentation option.</span></span>

    <span data-ttu-id="32b9e-315">![Параметры редактора HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "параметров редактора HTML")</span><span class="sxs-lookup"><span data-stu-id="32b9e-315">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="32b9e-316">*Параметры редактора HTML*</span><span class="sxs-lookup"><span data-stu-id="32b9e-316">*HTML Editor settings*</span></span>
2. <span data-ttu-id="32b9e-317">На **Default.aspx** страницы, удалите все содержимое в элементе аудио.</span><span class="sxs-lookup"><span data-stu-id="32b9e-317">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="32b9e-318">Поместите курсор в конце открывающий **аудио** элемент и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-318">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="32b9e-319">Обратите внимание, что новое положение курсора, уровень дополнительных отступов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-319">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="32b9e-320">![Отступ в редакторе HTML для смарт-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "интеллектуальных отступов в редакторе HTML")</span><span class="sxs-lookup"><span data-stu-id="32b9e-320">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="32b9e-321">*Интеллектуальных отступов в редакторе HTML*</span><span class="sxs-lookup"><span data-stu-id="32b9e-321">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="32b9e-322">Восстановление аудио тега с содержимым были удалены, или закройте **Default.aspx** без сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="32b9e-322">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="32b9e-323">Задача 6 - извлечение в пользовательский элемент управления</span><span class="sxs-lookup"><span data-stu-id="32b9e-323">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="32b9e-324">Рефакторинг средства включены в Visual Studio, таких как извлечение части кода в функцию, возможности, упрощающие улучшение и оптимизации существующего кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-324">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="32b9e-325">Аналог для страниц ASP.NET будет извлечение в пользовательский элемент управления HTML-код.</span><span class="sxs-lookup"><span data-stu-id="32b9e-325">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="32b9e-326">Таким же образом вручную будет включать в себя несколько шагов, таких как создание нового пользовательского элемента управления, перемещения раздела кода в пользовательский элемент управления, регистрация префикс тега для пользовательского элемента управления и, наконец, при создании экземпляра пользовательского элемента управления на страницах.</span><span class="sxs-lookup"><span data-stu-id="32b9e-326">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="32b9e-327">Теперь новый *извлечение в пользовательский элемент управления* средство автоматически выполняет эти шаги для вас.</span><span class="sxs-lookup"><span data-stu-id="32b9e-327">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="32b9e-328">В этой задаче будет использовать новый извлечение в пользовательский элемент управления контекстные операции для создания нового пользовательского элемента управления из выбранного кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-328">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="32b9e-329">На **Default.aspx** выберите **H2** и **аудио** элементов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-329">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="32b9e-330">Щелкните правой кнопкой мыши и выберите **извлечение в пользовательский элемент управления**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-330">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="32b9e-331">![Извлечение в пользовательский элемент управления меню параметр](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "извлечение в пользовательский элемент управления меню")</span><span class="sxs-lookup"><span data-stu-id="32b9e-331">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="32b9e-332">*Извлечение в пользовательский элемент управления меню*</span><span class="sxs-lookup"><span data-stu-id="32b9e-332">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="32b9e-333">Введите имя для нового пользовательского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="32b9e-333">Type a name for the new user control.</span></span> <span data-ttu-id="32b9e-334">Например **Jukebox.ascx**, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-334">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="32b9e-335">![Сохранение извлеченных пользовательский элемент управления](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "сохранение извлеченных пользовательского элемента управления")</span><span class="sxs-lookup"><span data-stu-id="32b9e-335">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="32b9e-336">*Сохранение извлеченных пользовательского элемента управления*</span><span class="sxs-lookup"><span data-stu-id="32b9e-336">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="32b9e-337">Обратите внимание, что выделенный код были извлечены в пользовательский элемент управления исходное расположение выбранного кода было заменено значением экземпляр нового пользовательского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="32b9e-337">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="32b9e-338">![Страница автоматически обновляются для использования нового пользовательского элемента управления](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "страницы автоматически обновляются для использования нового пользовательского элемента управления")</span><span class="sxs-lookup"><span data-stu-id="32b9e-338">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="32b9e-339">*Страница автоматически обновляются для использования нового пользовательского элемента управления*</span><span class="sxs-lookup"><span data-stu-id="32b9e-339">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="32b9e-340">Нажмите клавишу **F5** для запуска страницы и убедитесь, что он работает.</span><span class="sxs-lookup"><span data-stu-id="32b9e-340">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="32b9e-341">Упражнение 3: Новые возможности редактора JavaScript</span><span class="sxs-lookup"><span data-stu-id="32b9e-341">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="32b9e-342">Написания и редактирования кода JavaScript не является сложной задачей, особенно в том случае, когда приложение начинает увеличиваться в размере, и вы обнаружите задействования много файлов, а также сотни функций.</span><span class="sxs-lookup"><span data-stu-id="32b9e-342">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="32b9e-343">Сценарий разработчики обычно необходимо выполнить некоторые дополнительные действия для поддержки читаемость кода и перемещаться между файлами.</span><span class="sxs-lookup"><span data-stu-id="32b9e-343">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="32b9e-344">При включении библиотек JavaScript, таких как jQuery скрипт навигации стала самой сложной задачей из-за длина кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-344">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="32b9e-345">Редактор JavaScript с обязательством вносить режим кода доступны и организованными обновил Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-345">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="32b9e-346">В редакторе JavaScript теперь реализованы многие возможности Visual Studio, уже существующих в C# или VB редакторы: перейти к определению, автоматический отступ, документация и проверки при его написании.</span><span class="sxs-lookup"><span data-stu-id="32b9e-346">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="32b9e-347">С обновленной список IntelliSense каталога функции JavaScript будет иметь под рукой.</span><span class="sxs-lookup"><span data-stu-id="32b9e-347">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="32b9e-348">В этом упражнении вы узнаете о некоторых новых возможностей и усовершенствований редактора JavaScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-348">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="32b9e-349">Будет найдите файлы образцов и обнаружение всех новых характеристики, которые будут программирования JavaScript повышают эффективность в Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="32b9e-349">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="32b9e-350">Задача 1. новые возможности редактора JavaScript</span><span class="sxs-lookup"><span data-stu-id="32b9e-350">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="32b9e-351">Эта задача приведены некоторые новые возможности редактора JavaScript, которые нацелены на размещение кода и перевод улучшить взаимодействие с пользователем.</span><span class="sxs-lookup"><span data-stu-id="32b9e-351">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="32b9e-352">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="32b9e-352">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="32b9e-353">Нажмите клавишу **F5** для запуска приложения, щелкните ссылку "JavaScript" на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="32b9e-353">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="32b9e-354">Обновите страницу несколько раз и проверьте способ увеличения значения счетчика.</span><span class="sxs-lookup"><span data-stu-id="32b9e-354">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="32b9e-355">![Счетчик страниц](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "счетчика страницы")</span><span class="sxs-lookup"><span data-stu-id="32b9e-355">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="32b9e-356">*Счетчик страниц*</span><span class="sxs-lookup"><span data-stu-id="32b9e-356">*Page counter*</span></span>
3. <span data-ttu-id="32b9e-357">Закройте окно браузера и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-357">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="32b9e-358">Откройте **JavaScript.aspx** страницы и найдите **&lt;сценарий&gt;** блока (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="32b9e-358">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="32b9e-359">В следующем коде используется HTML5 локальное хранилище для хранения *pageLoadCount* переменной, которая хранит количество посещений страницы для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="32b9e-359">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="32b9e-360">Локальное хранилище база данных клиентского ключ значение впервые появилась в стандарте HTML5.</span><span class="sxs-lookup"><span data-stu-id="32b9e-360">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="32b9e-361">Данные сохраняются на локальном компьютере, в браузере пользователя.</span><span class="sxs-lookup"><span data-stu-id="32b9e-361">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="32b9e-362">Убедитесь, что DOCTYPE равно XHTML5 перед продолжением следующие шаги.</span><span class="sxs-lookup"><span data-stu-id="32b9e-362">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="32b9e-363">Измените код и обратите внимание, IntelliSense для JavaScript включает функции HTML5, такие как локальное хранилище и внутренние методы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-363">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="32b9e-364">![Функции HTML5 JavaScript в JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript возможностей в JavaScript")</span><span class="sxs-lookup"><span data-stu-id="32b9e-364">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="32b9e-365">*Функции HTML5 JavaScript в JavaScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-365">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="32b9e-366">Щелкните любой открывающую квадратную скобку (**{**) из сценариев кода и обратите внимание, что скобки выделяются.</span><span class="sxs-lookup"><span data-stu-id="32b9e-366">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="32b9e-367">![Квадратные скобки, выделяются](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "выделяются квадратные скобки")</span><span class="sxs-lookup"><span data-stu-id="32b9e-367">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="32b9e-368">*Квадратные скобки, выделяются.*</span><span class="sxs-lookup"><span data-stu-id="32b9e-368">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="32b9e-369">Раскомментируйте функция **testAutoAlign()** (выберите три строки и воспользуйтесь **CTRL** + **K**; **CTRL** + **U**) и найдите курсор внутри кода функции.</span><span class="sxs-lookup"><span data-stu-id="32b9e-369">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="32b9e-370">Нажмите клавишу ВВОД для добавления второй строки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-370">Press enter to append a second line.</span></span> <span data-ttu-id="32b9e-371">Обратите внимание, что код **выравниваются** и **с отступом автоматически**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-371">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="32b9e-372">![Код JavaScript будет автоматически выравниваются](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "код JavaScript будет автоматически выровнен")</span><span class="sxs-lookup"><span data-stu-id="32b9e-372">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="32b9e-373">*Код JavaScript будет автоматически выровнен*</span><span class="sxs-lookup"><span data-stu-id="32b9e-373">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="32b9e-374">Задача 2 - Проверка JavaScript</span><span class="sxs-lookup"><span data-stu-id="32b9e-374">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="32b9e-375">В этой задаче будет обнаруживать новые проверки JavaScript для стандартных ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="32b9e-375">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="32b9e-376">Эта возможность поможет вам создать совместимый код JavaScript, при Предотвращение сценарных перед развертыванием сайта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-376">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="32b9e-377">Visual Studio 2010 реализован соответствия ECMAStript3 хотя Visual Studio 2012 обеспечивает ECMAScript5 соответствия.</span><span class="sxs-lookup"><span data-stu-id="32b9e-377">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="32b9e-378">Откройте **ECMA5script5.js** в компоненте **Scripts\custom** папки проекта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-378">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="32b9e-379">Теперь можно протестировать проверки ECMAScript5 standard.</span><span class="sxs-lookup"><span data-stu-id="32b9e-379">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="32b9e-380">Можно извлечь &quot; **используйте strict** &quot; направление, в первой строке файла, который позволяет ECMAScript5 **строгий режим**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-380">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="32b9e-381">В этом режиме состоит в подмножестве языка, поясняющий неоднозначности от последние версии, а также добавляет некоторые новые возможности, такие как методы получения и задания, поддержку библиотеки для JSON, а также более полный отражения на свойствах объектов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-381">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="32b9e-382">Откройте **список ошибок** Если еще не открыт (**представление** меню | **Список ошибок**).</span><span class="sxs-lookup"><span data-stu-id="32b9e-382">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="32b9e-383">Обратите внимание **функция** подчеркнуты объявления.</span><span class="sxs-lookup"><span data-stu-id="32b9e-383">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="32b9e-384">Это так, как в ECMA5 стандартные функции не могут располагаться внутри структуры языка.</span><span class="sxs-lookup"><span data-stu-id="32b9e-384">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="32b9e-385">В сообщении об ошибке в списке вы увидите сведения о предупреждении.</span><span class="sxs-lookup"><span data-stu-id="32b9e-385">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="32b9e-386">![Сообщение об ошибке проверки JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "сообщений об ошибках проверки JavaScript")</span><span class="sxs-lookup"><span data-stu-id="32b9e-386">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="32b9e-387">*Сообщение об ошибке проверки JavaScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-387">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="32b9e-388">Закомментируйте **&quot;используйте strict&quot;** направление и обратите внимание, что ошибки исчезнут, но остаются предупреждения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-388">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="32b9e-389">В последней строке файла, записать любую строку как **&quot;тестирования&quot;** (заключать в кавычки, чтобы указать это, как строка).</span><span class="sxs-lookup"><span data-stu-id="32b9e-389">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="32b9e-390">Запись периода рядом с строке, чтобы вывести список IntelliSense и выбрать **trim** параметр.</span><span class="sxs-lookup"><span data-stu-id="32b9e-390">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="32b9e-391">В стандартном ECMAScript5 строковые значения и переменные также иметь методы строк, определенные как trim, прописные буквы, для поиска и замены.</span><span class="sxs-lookup"><span data-stu-id="32b9e-391">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="32b9e-392">![Список IntelliSense в JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "список IntelliSense в JavaScript")</span><span class="sxs-lookup"><span data-stu-id="32b9e-392">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="32b9e-393">*Список IntelliSense в JavaScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-393">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="32b9e-394">Задача 3 - XML-документации для JavaScript</span><span class="sxs-lookup"><span data-stu-id="32b9e-394">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="32b9e-395">В этой задаче будет изучено возможностей Visual Studio для XML-документации в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-395">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="32b9e-396">Вы увидите, что в списке IntelliSense для JavaScript теперь отображаются в XML-документации для каждой функции.</span><span class="sxs-lookup"><span data-stu-id="32b9e-396">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="32b9e-397">Кроме того вы заметите компонент навигации в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-397">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="32b9e-398">Откройте **XMLDoc.js** файл, расположенный в **скриптов или настраиваемый** папки проекта.</span><span class="sxs-lookup"><span data-stu-id="32b9e-398">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="32b9e-399">Этот файл содержит XML-документацию для всех функций JavaScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-399">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="32b9e-400">![Интегрированное документация JavaScript XML IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "интегрированных JavaScript XML-документации для IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="32b9e-400">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="32b9e-401">*Документация JavaScript XML, интегрированные в IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="32b9e-401">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="32b9e-402">Ниже **добавить** функционировать в **XMLDoc.js** файла, создайте новую функцию с именем **тестирования**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-402">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="32b9e-403">В **тестирования** вызовите метод **умножить** функцию, которая принимает два параметра.</span><span class="sxs-lookup"><span data-stu-id="32b9e-403">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="32b9e-404">Обратите внимание на то, отображается окно всплывающей подсказки **умножить** функции документации.</span><span class="sxs-lookup"><span data-stu-id="32b9e-404">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="32b9e-405">![XML-документации для функций JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML-документации для функций JavaScript")</span><span class="sxs-lookup"><span data-stu-id="32b9e-405">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="32b9e-406">*XML-документации для функций JavaScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-406">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="32b9e-407">Выполните оператор вызова функции и тип *точка* чтобы открыть список IntelliSense на возвращаемое значение.</span><span class="sxs-lookup"><span data-stu-id="32b9e-407">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="32b9e-408">Обратите внимание, что обнаружение Visual Studio **возвращают** значение в документации по обработке значение как число.</span><span class="sxs-lookup"><span data-stu-id="32b9e-408">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="32b9e-409">![XML-документацию для возвращаемых типов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML-документацию для возвращаемых типов")</span><span class="sxs-lookup"><span data-stu-id="32b9e-409">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="32b9e-410">*XML-документацию для возвращаемых типов*</span><span class="sxs-lookup"><span data-stu-id="32b9e-410">*XML documentation for return types*</span></span>
5. <span data-ttu-id="32b9e-411">Теперь вставьте вызов, чтобы добавить функцию.</span><span class="sxs-lookup"><span data-stu-id="32b9e-411">Now, insert a call to add function.</span></span> <span data-ttu-id="32b9e-412">Обратите внимание, что редактор JavaScript теперь поддерживает перегрузки функций.</span><span class="sxs-lookup"><span data-stu-id="32b9e-412">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="32b9e-413">При создании имени функции, можно выбрать любой из доступных перегруженных функций, указанное в документации.</span><span class="sxs-lookup"><span data-stu-id="32b9e-413">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="32b9e-414">![XML-документации для перегрузок](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML-документацию для перегрузки")</span><span class="sxs-lookup"><span data-stu-id="32b9e-414">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="32b9e-415">*XML-документацию для перегрузки*</span><span class="sxs-lookup"><span data-stu-id="32b9e-415">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="32b9e-416">Откройте **GotoDefinition.js** и найдите **$().html()** вызов функции.</span><span class="sxs-lookup"><span data-stu-id="32b9e-416">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="32b9e-417">Найдите курсор на **html**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-417">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="32b9e-418">Нажмите клавишу **F12** и перейти к определению.</span><span class="sxs-lookup"><span data-stu-id="32b9e-418">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="32b9e-419">Обратите внимание, теперь можно открыть и просмотреть код JavaScript, без использования **найти** средства.</span><span class="sxs-lookup"><span data-stu-id="32b9e-419">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="32b9e-420">Найдите курсор на инструкцию jQuery до блок подписи в нижней части файла кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-420">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="32b9e-421">Нажмите клавишу **F12**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-421">Press **F12**.</span></span> <span data-ttu-id="32b9e-422">Переместитесь к файлу библиотеки jQuery.</span><span class="sxs-lookup"><span data-stu-id="32b9e-422">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="32b9e-423">Обратите внимание, можно также перейти в файлах jQuery, с помощью **F12**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-423">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="32b9e-424">![Перехода к определениям jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "перехода к определениям jQuery")</span><span class="sxs-lookup"><span data-stu-id="32b9e-424">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="32b9e-425">*Перехода к определениям jQuery*</span><span class="sxs-lookup"><span data-stu-id="32b9e-425">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="32b9e-426">Убедитесь, что GotoDefinition.js не содержат синтаксических ошибок до сохранения файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-426">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="32b9e-427">Упражнение 4: Объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="32b9e-427">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="32b9e-428">Сколько раз следует веб-сайты включать несколько JavaScript и CSS-файл?</span><span class="sxs-lookup"><span data-stu-id="32b9e-428">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="32b9e-429">Это очень распространенный сценарий, где объединение и Минификация помогают уменьшить размер файла и сделать сайт работать быстрее.</span><span class="sxs-lookup"><span data-stu-id="32b9e-429">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="32b9e-430">Объединение в новый компонент ASP.NET 4.5 пакеты набор файлов JS или CSS в один элемент и уменьшает его размер, Минификация содержимое (т. е. Удаление пробелов не требуются, удаление комментариев, уменьшая идентификаторов).</span><span class="sxs-lookup"><span data-stu-id="32b9e-430">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="32b9e-431">Объединение и Минификация в ASP.NET 4.5 выполняется во время выполнения, чтобы процесс идентификации агент пользователя (например IE, Mozilla, и т. д.) и таким образом, улучшить сжатие, предназначенных для браузера пользователя (для экземпляра удаление stuff, Mozilla конкретных При поступлении запроса на из IE).</span><span class="sxs-lookup"><span data-stu-id="32b9e-431">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="32b9e-432">В этом упражнении вы узнаете, как включить и использовать различные типы объединение и Минификация в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="32b9e-432">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="32b9e-433">Задача 1. Установка объединение и Минификация пакета из NuGet</span><span class="sxs-lookup"><span data-stu-id="32b9e-433">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="32b9e-434">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="32b9e-434">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="32b9e-435">Откройте консоль диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="32b9e-435">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="32b9e-436">Чтобы сделать это, используйте меню **представление** | **другие окна** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-436">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="32b9e-437">![Открытие file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole диспетчера пакетов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Открытие консоли диспетчера пакетов")</span><span class="sxs-lookup"><span data-stu-id="32b9e-437">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="32b9e-438">*Открытие консоли диспетчера пакетов*</span><span class="sxs-lookup"><span data-stu-id="32b9e-438">*Opening the package manager console*</span></span>
3. <span data-ttu-id="32b9e-439">В **консоль диспетчера пакетов** тип **Microsoft.Web.Optimization Install-Package** и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-439">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="32b9e-440">Задача 2 - по умолчанию пакеты</span><span class="sxs-lookup"><span data-stu-id="32b9e-440">Task 2 - Default Bundles</span></span>

<span data-ttu-id="32b9e-441">Чтобы использовать объединение и Минификация проще всего включить пакеты по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32b9e-441">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="32b9e-442">Этот метод использует соглашения, чтобы можно было ссылаться на распространение и уменьшенный версию файлов в папке JS- и CSS.</span><span class="sxs-lookup"><span data-stu-id="32b9e-442">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="32b9e-443">В этой задаче вы узнаете, как включить и ссылки на распространение и уменьшенный JS- и CSS файлы и просмотрите результаты.</span><span class="sxs-lookup"><span data-stu-id="32b9e-443">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="32b9e-444">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="32b9e-444">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="32b9e-445">В **обозревателе решений**, разверните **стили**, **Scripts\custom** и **Scripts\bundle** папки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-445">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="32b9e-446">Обратите внимание, что приложение использует несколько CSS и JS-файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-446">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="32b9e-447">![Файлы на несколько таблиц стилей и JavaScript в приложении](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "файлов несколько таблиц стилей и JavaScript в приложении")</span><span class="sxs-lookup"><span data-stu-id="32b9e-447">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="32b9e-448">*Несколько файлов таблицы стилей и JavaScript в приложении*</span><span class="sxs-lookup"><span data-stu-id="32b9e-448">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="32b9e-449">Откройте **Global.asax.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-449">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="32b9e-450">Обратите внимание, что новый **Microsoft.Web.Optimization** закомментирован пространства имен в начале файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-450">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="32b9e-451">Удалите метки комментария с помощью директивы объединение и Минификация возможности.</span><span class="sxs-lookup"><span data-stu-id="32b9e-451">Uncomment the using directive to include the bundling and minification features.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
~~~
4. <span data-ttu-id="32b9e-452">Найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="32b9e-452">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="32b9e-453">В этом методе раскомментируйте EnableDefaultBundles вызова, как показано в приведенном ниже фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-453">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="32b9e-454">Это позволяет ссылаться на объединенной коллекции файлов CSS в папке, используя путь к этой папке, а также &quot;CSS&quot; или &quot;JS&quot; суффикс.</span><span class="sxs-lookup"><span data-stu-id="32b9e-454">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
~~~
5. <span data-ttu-id="32b9e-455">Откройте **Optimization.aspx** и найдите элемент управления содержимым для **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-455">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="32b9e-456">Обратите внимание, что файлы CSS и JS содержать единственный тег на которую указывает ссылка.</span><span class="sxs-lookup"><span data-stu-id="32b9e-456">Notice the CSS files and the JS files have a single referenced tag.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

> [!NOTE]
> This code is for demo purposes. Ideally, you will reference the bundles in the Site.Master file. In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.
~~~
6. <span data-ttu-id="32b9e-457">Обратите внимание, что ссылки с помощью объединение соглашения в **href** атрибут для получения файлов CSS и JS из стили и Scripts\custom папки соответственно.</span><span class="sxs-lookup"><span data-stu-id="32b9e-457">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="32b9e-458">Можно использовать путь **сценарии или пользовательский/JS** как показано ниже, для объединения и уменьшения файлы JS внутри **скриптов или настраиваемый** папки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-458">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="32b9e-459">Это поведение по умолчанию с помощью пакетов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32b9e-459">This is the default behavior with the default bundles.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
~~~
7. <span data-ttu-id="32b9e-460">Откройте **Styles\Site.css** файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-460">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="32b9e-461">Обратите внимание, что исходный CSS-файл содержит код с отступами, пробелы и комментарии, увеличить размер файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-461">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="32b9e-462">(Также файл JavaScript содержит пробелы и комментарии).</span><span class="sxs-lookup"><span data-stu-id="32b9e-462">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="32b9e-463">![Один из исходного CSS файлов в папку «скрипты»](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "один исходный CSS файлы в папку «скрипты»")</span><span class="sxs-lookup"><span data-stu-id="32b9e-463">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="32b9e-464">*Один из исходных файлов CSS в папку «скрипты»*</span><span class="sxs-lookup"><span data-stu-id="32b9e-464">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="32b9e-465">Нажмите клавишу **F5** для запуска приложения и перехода к **оптимизации** страницы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-465">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="32b9e-466">Щелкните **пакета CSS** ссылку для загрузки и откройте файл.</span><span class="sxs-lookup"><span data-stu-id="32b9e-466">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="32b9e-467">Извлечь уменьшенный объединенный файл.</span><span class="sxs-lookup"><span data-stu-id="32b9e-467">Check out the minified bundled file.</span></span> <span data-ttu-id="32b9e-468">Можно заметить, что были удалены все пробелы, комментарии и символы отступы, созданию файла меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="32b9e-468">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="32b9e-469">![Объединенными файлами CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "объединенными CSS-файлах")</span><span class="sxs-lookup"><span data-stu-id="32b9e-469">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="32b9e-470">*Распространение файлов CSS*</span><span class="sxs-lookup"><span data-stu-id="32b9e-470">*Bundled CSS files*</span></span>
10. <span data-ttu-id="32b9e-471">Теперь щелкните **пакета JS** ссылку, чтобы открыть файл JavaScript, которые объединяются в один пакет.</span><span class="sxs-lookup"><span data-stu-id="32b9e-471">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="32b9e-472">Проводник предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="32b9e-472">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="32b9e-473">Обратите внимание, файлы JavaScript с использованием **пользовательские** папки также объединяются в один пакет и уменьшена.</span><span class="sxs-lookup"><span data-stu-id="32b9e-473">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="32b9e-474">![Упакованные файлы JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "файлы JavaScript, объединенными")</span><span class="sxs-lookup"><span data-stu-id="32b9e-474">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="32b9e-475">*Пакетные файлы JavaScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-475">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="32b9e-476">Включение сжатия для файлов CSS и JS был значительно сложнее в предыдущей версии ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32b9e-476">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="32b9e-477">Теперь, как вы уже видели, необходимо просто добавить одну строку в *Global.asax* файл, чтобы включить объединение, а затем ссылка объединенными файлами с веб-узла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-477">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="32b9e-478">Задача 3 - статических пакетов</span><span class="sxs-lookup"><span data-stu-id="32b9e-478">Task 3 - Static Bundles</span></span>

<span data-ttu-id="32b9e-479">Подход с использованием статических пакета позволяет настраивать набор файлов для пакета, ссылки и Минификация метод, который будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="32b9e-479">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="32b9e-480">В этой задаче вы настроите статический набор, определяется конкретный набор файлов для объединения и уменьшения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-480">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="32b9e-481">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="32b9e-481">Close the browser.</span></span>
2. <span data-ttu-id="32b9e-482">Откройте **Global.asax.cs** и найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="32b9e-482">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="32b9e-483">Раскомментируйте код статических пакета, как показано в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-483">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="32b9e-484">Вы определяете статических пакет, который будет ссылаться с &quot; **~/StaticBundle** &quot; виртуальный путь и используйте **JsMinify** для всех указанных файлов с Минификация **AddFile** метод.</span><span class="sxs-lookup"><span data-stu-id="32b9e-484">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="32b9e-485">Наконец, вы добавляете статический набор **BundleTable** и включить его.</span><span class="sxs-lookup"><span data-stu-id="32b9e-485">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="32b9e-486">Обратите внимание, что файлы не находятся в том же месте; Это еще одно преимущество над объединение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="32b9e-486">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
~~~
4. <span data-ttu-id="32b9e-487">Откройте **Optimization.aspx** файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-487">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="32b9e-488">Обратите внимание, что эту ссылку, чтобы **статических JS пакета** используется путь, объявленные при настройке статического пакета в файле Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-488">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
~~~
5. <span data-ttu-id="32b9e-489">Нажмите клавишу **F5** для запуска приложения, а затем перейдите к **оптимизации** страницы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-489">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="32b9e-490">Щелкните **статических JS пакета** ссылку, чтобы открыть файл.</span><span class="sxs-lookup"><span data-stu-id="32b9e-490">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="32b9e-491">Обратите внимание, что уменьшенный объединенными файл JavaScript — это выходной файл для всех файлов JavaScript, заданный в файле статических пакет по пути &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="32b9e-491">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="32b9e-492">![Статический набор файлов JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "JavaScript статические файлы пакета")</span><span class="sxs-lookup"><span data-stu-id="32b9e-492">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="32b9e-493">*Объединить статических файлов JavaScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-493">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="32b9e-494">Закройте окно браузера и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-494">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="32b9e-495">Задача 4 - папке динамических пакетов</span><span class="sxs-lookup"><span data-stu-id="32b9e-495">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="32b9e-496">В этой задаче вы узнаете, как настроить папку динамических пакетов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-496">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="32b9e-497">Динамическое объединение является, можно включить статический JavaScript, а также другие файлы на языках, компилируемый в JavaScript и таким образом, требуются некоторые обработки перед выполнением объединения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-497">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="32b9e-498">В этом примере вы узнаете, как использовать **DynamicFolderBundle** класса для создания динамического пакета для файлов, записанных в CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-498">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="32b9e-499">CofeeScript — это язык программирования, который компилируемый в JavaScript и предоставляет более простой синтаксис для написания кода JavaScript, что повышает краткости и удобочитаемости кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-499">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="32b9e-500">Откройте **Global.asax.cs** и найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="32b9e-500">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="32b9e-501">Раскомментируйте код динамического пакета, как показано в приведенном ниже примере кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-501">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="32b9e-502">Вы определяете пакета динамического папки, в которую будет использовать **CoffeeMinify** пользовательские минификации процессором, который будет применяться только к файлам с &quot; **.coffee** &quot; (расширения CoffeeScript-файлы).</span><span class="sxs-lookup"><span data-stu-id="32b9e-502">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="32b9e-503">Обратите внимание, можно использовать шаблон поиска для выбора файлов для формирования пакета в папке, например "\*.coffee".</span><span class="sxs-lookup"><span data-stu-id="32b9e-503">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
~~~
3. <span data-ttu-id="32b9e-504">Откройте консоль диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="32b9e-504">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="32b9e-505">Чтобы сделать это, используйте меню **представление** | **другие окна** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-505">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="32b9e-506">В **консоль диспетчера пакетов** тип **CoffeeSharp Install-Package** и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-506">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="32b9e-507">Нажмите кнопку **Показать все файлы** кнопку в **обозревателе решений** окна</span><span class="sxs-lookup"><span data-stu-id="32b9e-507">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="32b9e-508">![Отображение всех файлов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "отображение всех файлов")</span><span class="sxs-lookup"><span data-stu-id="32b9e-508">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="32b9e-509">*Отображение всех файлов*</span><span class="sxs-lookup"><span data-stu-id="32b9e-509">*Showing all files*</span></span>
6. <span data-ttu-id="32b9e-510">Щелкните правой кнопкой мыши **CoffeeMinify.cs** файла в **обозревателе решений** и выберите **включить в проект**</span><span class="sxs-lookup"><span data-stu-id="32b9e-510">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="32b9e-511">![Включить в проект файл CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "включить в проект файл CoffeeMinify.cs")</span><span class="sxs-lookup"><span data-stu-id="32b9e-511">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="32b9e-512">*Включить в проект файл CoffeeMinify.cs*</span><span class="sxs-lookup"><span data-stu-id="32b9e-512">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="32b9e-513">Откройте **CoffeeMinify.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-513">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="32b9e-514">Этот класс наследует от JsMinify для уменьшения выходные данные JavaScript, возникающие в результате компиляции кода CoffeeScript.</span><span class="sxs-lookup"><span data-stu-id="32b9e-514">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="32b9e-515">Он вызывает CoffeeScript компилятору создавать код JavaScript, сначала и затем он отправляет методу JsMinify.Process для уменьшения конечный код.</span><span class="sxs-lookup"><span data-stu-id="32b9e-515">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
~~~
8. <span data-ttu-id="32b9e-516">Откройте **Script1.coffee** и **Script2.coffee** файлов из **скриптов или пакета** папки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-516">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="32b9e-517">Эти файлы содержат код CoffeScript для компиляции во время выполнения объединения с классом CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="32b9e-517">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="32b9e-518">В целях упрощения предоставленных файлах CoffeeScript только включаете CoffeeScript образец кода.</span><span class="sxs-lookup"><span data-stu-id="32b9e-518">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="32b9e-519">Комментарии исключаются JsMinify процессом.</span><span class="sxs-lookup"><span data-stu-id="32b9e-519">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="32b9e-520">![Файлы CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript файлов")</span><span class="sxs-lookup"><span data-stu-id="32b9e-520">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="32b9e-521">*Файлы CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="32b9e-521">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) предоставляет более простой синтаксис для написания кода JavaScript, улучшение краткости и удобочитаемости кода JavaScript, а также добавление оператор like понимания массива и другие функции.</span><span class="sxs-lookup"><span data-stu-id="32b9e-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="32b9e-523">Откройте **Optimization.aspx** и найдите пакет ссылки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-523">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="32b9e-524">Обратите внимание, что эту ссылку, чтобы **динамического пакета JS** ссылается **скриптов или пакета** папки с помощью **/кофе** настроен для пакета папки динамического суффикс.</span><span class="sxs-lookup"><span data-stu-id="32b9e-524">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
~~~
10. <span data-ttu-id="32b9e-525">Нажмите клавишу **F5** для запуска приложения, а затем перейдите к **оптимизации** страницы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-525">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="32b9e-526">Щелкните **динамического пакета JS** ссылку, чтобы открыть созданный файл.</span><span class="sxs-lookup"><span data-stu-id="32b9e-526">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="32b9e-527">Обратите внимание, что содержимое, которое было включено в этот пакет содержит только **.coffee** файлов.</span><span class="sxs-lookup"><span data-stu-id="32b9e-527">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="32b9e-528">Также вы увидите, что CoffeeScript код был скомпилирован в код JavaScript и закомментированных строк было удалено.</span><span class="sxs-lookup"><span data-stu-id="32b9e-528">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="32b9e-529">![Объединить динамических JS-файлов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS динамические файлы пакета")</span><span class="sxs-lookup"><span data-stu-id="32b9e-529">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="32b9e-530">*Объединить динамических JS-файлов*</span><span class="sxs-lookup"><span data-stu-id="32b9e-530">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="32b9e-531">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="32b9e-531">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="32b9e-532">Сводка</span><span class="sxs-lookup"><span data-stu-id="32b9e-532">Summary</span></span>

<span data-ttu-id="32b9e-533">Эта лаборатория поможет вам понять, что такое новые возможности ASP.NET и веб-разработки в Visual Studio 2012 и как использовать преимущества различных улучшений в Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="32b9e-533">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="32b9e-534">Выполнив это Практическое лабораторное занятие, выученных, как использовать новые функции и усовершенствования в Visual Studio 2012 редакторы CSS, JavaScript и HTML.</span><span class="sxs-lookup"><span data-stu-id="32b9e-534">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="32b9e-535">Кроме того выученных, как Visual Studio 2012 реализует встроенных объединение и Минификация.</span><span class="sxs-lookup"><span data-stu-id="32b9e-535">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="32b9e-536">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="32b9e-536">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="32b9e-537">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-537">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="32b9e-538">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="32b9e-538">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="32b9e-539">Последовательно выберите пункты [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="32b9e-539">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="32b9e-540">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="32b9e-540">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="32b9e-541">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-541">Click on **Install Now**.</span></span> <span data-ttu-id="32b9e-542">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="32b9e-542">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="32b9e-543">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-543">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="32b9e-544">![Установка Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="32b9e-544">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="32b9e-545">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="32b9e-545">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="32b9e-546">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-546">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="32b9e-548">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="32b9e-548">*Accepting the license terms*</span></span>
5. <span data-ttu-id="32b9e-549">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-549">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="32b9e-551">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="32b9e-551">*Installation progress*</span></span>
6. <span data-ttu-id="32b9e-552">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-552">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="32b9e-554">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="32b9e-554">*Installation completed*</span></span>
7. <span data-ttu-id="32b9e-555">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="32b9e-555">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="32b9e-556">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="32b9e-556">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="32b9e-558">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="32b9e-558">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="32b9e-559">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="32b9e-559">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="32b9e-560">В этом приложении будет показано, как создать новый веб-сайт на портале управления Windows Azure и публикация приложения, получить, выполнив в лаборатории преимуществами средство публикации веб-развертывания, предоставленного Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="32b9e-560">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="32b9e-561">Задача 1. Создание нового веб-узла с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="32b9e-561">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="32b9e-562">Последовательно выберите пункты [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="32b9e-562">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-563">С помощью Windows Azure можно бесплатно размещения 10 веб-сайтов ASP.NET и затем масштабирование по мере увеличения трафика.</span><span class="sxs-lookup"><span data-stu-id="32b9e-563">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="32b9e-564">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="32b9e-564">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="32b9e-565">![Войдите на портал Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "входа на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="32b9e-565">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="32b9e-566">*Выполните вход на портале управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="32b9e-566">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="32b9e-567">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="32b9e-567">Click **New** on the command bar.</span></span>

    <span data-ttu-id="32b9e-568">![Создание нового веб-сайта](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="32b9e-568">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="32b9e-569">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="32b9e-569">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="32b9e-570">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-570">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="32b9e-571">Выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="32b9e-571">Then select **Quick Create** option.</span></span> <span data-ttu-id="32b9e-572">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **Создание веб-сайта**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-572">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-573">Веб-приложение, работающее в облаке, вы можете управлять размещается веб-сайта Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="32b9e-573">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="32b9e-574">Параметр быстрого создания позволяет развернуть завершенное веб-приложение для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="32b9e-574">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="32b9e-575">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="32b9e-575">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="32b9e-576">![Создание нового веб-сайта с помощью быстрого создания](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "создания нового веб-сайта с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="32b9e-576">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="32b9e-577">*Создание нового веб-сайта с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="32b9e-577">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="32b9e-578">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="32b9e-578">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="32b9e-579">После создания веб-сайта по ссылке **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="32b9e-579">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="32b9e-580">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="32b9e-580">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="32b9e-581">![Просмотр новых веб-сайт](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="32b9e-581">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="32b9e-582">*Просмотр новых веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="32b9e-582">*Browsing to the new web site*</span></span>

    <span data-ttu-id="32b9e-583">![Веб-сайта под управлением](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "веб-сайта под управлением")</span><span class="sxs-lookup"><span data-stu-id="32b9e-583">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="32b9e-584">*Запуск веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="32b9e-584">*Web site running*</span></span>
6. <span data-ttu-id="32b9e-585">Вернитесь на портал и щелкните имя веб-сайта в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="32b9e-585">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="32b9e-586">![Открытие страницы управления веб-сайт](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Открытие страниц управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="32b9e-586">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="32b9e-587">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="32b9e-587">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="32b9e-588">В **панели мониторинга** в разделе **беглого** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="32b9e-588">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-589">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайте Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="32b9e-589">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="32b9e-590">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверки подлинности с каждой из конечных точек, для которых включен метода публикации.</span><span class="sxs-lookup"><span data-stu-id="32b9e-590">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="32b9e-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки этих программ для Публикация веб-приложений на веб-сайтов Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="32b9e-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="32b9e-592">![Профиль публикации веб-сайт загрузки](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="32b9e-592">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="32b9e-593">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="32b9e-593">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="32b9e-594">Загрузите файл профиля публикации в известном расположении.</span><span class="sxs-lookup"><span data-stu-id="32b9e-594">Download the publish profile file to a known location.</span></span> <span data-ttu-id="32b9e-595">Далее в этом упражнении вы увидите как использовать этот файл для публикации веб-приложения для веб-сайтов, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32b9e-595">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="32b9e-596">![Сохранить файл профиля публикации](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="32b9e-596">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="32b9e-597">*Сохранить файл профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="32b9e-597">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="32b9e-598">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="32b9e-598">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="32b9e-599">Если приложение использует SQL Server базы данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="32b9e-599">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="32b9e-600">Если вы хотите развернуть простое приложение, которое не использует SQL Server может пропустить эту задачу.</span><span class="sxs-lookup"><span data-stu-id="32b9e-600">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="32b9e-601">Сервер базы данных SQL необходимо для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-601">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="32b9e-602">Можно просматривать серверы базы данных SQL из подписки на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-602">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="32b9e-603">Если у вас на сервер, созданный, можно создать ее, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="32b9e-603">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="32b9e-604">Обратите внимание **имя сервера и URL-адрес, имя входа администратора и пароль**, как будет использовать их в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="32b9e-604">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="32b9e-605">Не следует создавать базу данных, как он будет создан в более поздней стадии.</span><span class="sxs-lookup"><span data-stu-id="32b9e-605">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="32b9e-606">![Панель мониторинга базы данных SQL Server](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="32b9e-606">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="32b9e-607">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="32b9e-607">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="32b9e-608">В следующей задаче требуется проверить подключение к базе данных из Visual Studio, по этой причине необходимо включить локальный IP-адреса в список серверов **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-608">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="32b9e-609">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля.</span><span class="sxs-lookup"><span data-stu-id="32b9e-609">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="32b9e-610">Введите имя для правила и нажмите кнопку ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="32b9e-610">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Добавление IP-адрес клиента](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="32b9e-612">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="32b9e-612">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="32b9e-613">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-613">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="32b9e-615">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="32b9e-615">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="32b9e-616">Задача 3. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="32b9e-616">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="32b9e-617">Вернитесь в решении ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="32b9e-617">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="32b9e-618">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-618">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="32b9e-619">![Публикация приложения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "публикация приложения")</span><span class="sxs-lookup"><span data-stu-id="32b9e-619">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="32b9e-620">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="32b9e-620">*Publishing the web site*</span></span>
2. <span data-ttu-id="32b9e-621">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="32b9e-621">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="32b9e-622">![Импорт профиля публикации](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="32b9e-622">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="32b9e-623">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="32b9e-623">*Importing publish profile*</span></span>
3. <span data-ttu-id="32b9e-624">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-624">Click **Validate Connection**.</span></span> <span data-ttu-id="32b9e-625">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-625">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32b9e-626">Проверка завершена, как только вы увидите зеленый флажок рядом с "Проверить подключение".</span><span class="sxs-lookup"><span data-stu-id="32b9e-626">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="32b9e-627">![Проверка подключения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="32b9e-627">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="32b9e-628">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="32b9e-628">*Validating connection*</span></span>
4. <span data-ttu-id="32b9e-629">В **параметры** в разделе **баз данных** статьи, нажмите кнопку рядом с текстового поля для подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="32b9e-629">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="32b9e-630">![Конфигурация веб-развертывания](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="32b9e-630">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="32b9e-631">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="32b9e-631">*Web deploy configuration*</span></span>
5. <span data-ttu-id="32b9e-632">Настройте подключение к базе данных следующим образом.</span><span class="sxs-lookup"><span data-stu-id="32b9e-632">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="32b9e-633">В **имя сервера** введите ваш URL-адрес сервера базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="32b9e-633">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="32b9e-634">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="32b9e-634">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="32b9e-635">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="32b9e-635">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="32b9e-636">Введите имя новой базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="32b9e-636">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="32b9e-637">![Настройка строки подключения назначения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Настройка целевой строки подключения")</span><span class="sxs-lookup"><span data-stu-id="32b9e-637">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="32b9e-638">*Настройка целевой строки подключения*</span><span class="sxs-lookup"><span data-stu-id="32b9e-638">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="32b9e-639">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-639">Then click **OK**.</span></span> <span data-ttu-id="32b9e-640">Когда появится запрос на создание базы данных щелкните **Да**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-640">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="32b9e-641">![Создание базы данных](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="32b9e-641">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="32b9e-642">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="32b9e-642">*Creating the database*</span></span>
7. <span data-ttu-id="32b9e-643">Строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure отображается внутри текстового поля по умолчанию соединения.</span><span class="sxs-lookup"><span data-stu-id="32b9e-643">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="32b9e-644">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-644">Then click **Next**.</span></span>

    <span data-ttu-id="32b9e-645">![Строка подключения, указывающий базу данных SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "строку соединения, указывающий базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="32b9e-645">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="32b9e-646">*Строка подключения, указывающий базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="32b9e-646">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="32b9e-647">В **предварительного просмотра** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="32b9e-647">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="32b9e-648">![Публикация веб-приложения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="32b9e-648">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="32b9e-649">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="32b9e-649">*Publishing the web application*</span></span>
9. <span data-ttu-id="32b9e-650">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-узла.</span><span class="sxs-lookup"><span data-stu-id="32b9e-650">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="32b9e-651">![Публикации приложения в Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="32b9e-651">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="32b9e-652">*Приложения, опубликованные в Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="32b9e-652">*Application published to Windows Azure*</span></span>
