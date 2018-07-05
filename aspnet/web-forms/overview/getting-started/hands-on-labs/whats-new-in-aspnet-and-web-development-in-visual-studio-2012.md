---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Новые возможности в ASP.NET и веб-разработки в Visual Studio 2012 | Документация Майкрософт
author: rick-anderson
description: Новая версия Visual Studio появился ряд усовершенствований, ориентированы на улучшение качества и производительности при работе с веб-технологиями...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d037ccb62693a07ab8e2640b2d930ec7093b35da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375889"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="e8f08-103">Новые возможности в ASP.NET и веб-разработки в Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e8f08-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="e8f08-104">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e8f08-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="e8f08-105">Новая версия Visual Studio представлен ряд усовершенствований, ориентированы на улучшение качества и производительности при работе с веб-технологиями.</span><span class="sxs-lookup"><span data-stu-id="e8f08-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="e8f08-106">Редакторы Visual Studio для CSS, JavaScript и HTML полностью переработан для включения многие из наиболее востребованные вспомогательные элементы кода, такие как IntelliSense и автоматического отступа.</span><span class="sxs-lookup"><span data-stu-id="e8f08-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="e8f08-107">Касающиеся производительности объединение и Минификация теперь интегрированы как встроенные функции, чтобы легко уменьшить страницы время загрузки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="e8f08-108">Visual Studio позволяет работать с последними технологиями веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="e8f08-109">Обозреватели CSS3 фрагменты можно использовать, чтобы убедиться в том, что ваш сайт работает независимо от платформы клиента одновременно пользуясь преимуществами новых элементов HTML5 и функции.</span><span class="sxs-lookup"><span data-stu-id="e8f08-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="e8f08-110">Написание и профилирование кода JavaScript должно быть проще с помощью этой версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="e8f08-111">Списки IntelliSense, интегрированная XML документации и функции навигации теперь доступны для кода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="e8f08-112">Теперь у вас есть каталог JavaScript в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="e8f08-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="e8f08-113">Кроме того можно проверить соответствие ECMAScript5 со сценариями и обнаружить синтаксические ошибки на раннем этапе.</span><span class="sxs-lookup"><span data-stu-id="e8f08-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="e8f08-114">Последнее, но не менее эта версия Visual Studio реализует встроенные объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="e8f08-115">Файлы скриптов и стилей будет упаковываются и сжимаются, чтобы сайт выполняется быстрее.</span><span class="sxs-lookup"><span data-stu-id="e8f08-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="e8f08-116">Это лабораторное занятие поможет усовершенствования и новые возможности, описанные ранее, применяя незначительные изменения в образец веб-приложение, в папке с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="e8f08-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="e8f08-117">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e8f08-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e8f08-118">Цели</span><span class="sxs-lookup"><span data-stu-id="e8f08-118">Objectives</span></span>

<span data-ttu-id="e8f08-119">В этом Практическое лабораторное занятие, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="e8f08-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="e8f08-120">Использовать новые функции и улучшения в редакторе CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="e8f08-121">Использовать новые функции и улучшения в редакторе HTML</span><span class="sxs-lookup"><span data-stu-id="e8f08-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="e8f08-122">Использовать новые функции и усовершенствования в редактор JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8f08-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="e8f08-123">Настройка и использование объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e8f08-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e8f08-124">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e8f08-124">Prerequisites</span></span>

- <span data-ttu-id="e8f08-125">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="e8f08-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="e8f08-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (для сценариев установки - уже установлен на Windows 8 и Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="e8f08-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="e8f08-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - или совместимые браузере HTML5</span><span class="sxs-lookup"><span data-stu-id="e8f08-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e8f08-128">Упражнения</span><span class="sxs-lookup"><span data-stu-id="e8f08-128">Exercises</span></span>

<span data-ttu-id="e8f08-129">Это Практическое лабораторное занятие включает в себя следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="e8f08-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e8f08-130">Упражнение 1: Новые возможности в редакторе CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="e8f08-131">Упражнение 2: Новые возможности в редакторе HTML</span><span class="sxs-lookup"><span data-stu-id="e8f08-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="e8f08-132">Упражнение 3: Новые возможности в редакторе JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8f08-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="e8f08-133">Упражнение 4: Объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e8f08-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="e8f08-134">Предполагаемое время для выполнения этого лабораторного: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="e8f08-135">Упражнение 1: Новые возможности в редакторе CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="e8f08-136">Веб-разработчики должны быть знакомы с множеством трудностей, связанных с CSS редактирования.</span><span class="sxs-lookup"><span data-stu-id="e8f08-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="e8f08-137">Одна из самых серьезных проблем Дизайн CSS — различными браузерами.</span><span class="sxs-lookup"><span data-stu-id="e8f08-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="e8f08-138">Часто бывает, что, после применения стилей к узлу, вы Обратите внимание, что она выглядит иначе при открытии в другой браузер или устройство.</span><span class="sxs-lookup"><span data-stu-id="e8f08-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="e8f08-139">Таким образом значительное время можно потратить на решение этих проблем visual осознавать, что, когда наконец оно работало в один браузер, она разбивается в других случаях.</span><span class="sxs-lookup"><span data-stu-id="e8f08-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="e8f08-140">Visual Studio теперь включает функции, которые помогают разработчикам доступ к, работы и эффективно организовать стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="e8f08-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="e8f08-141">В этом упражнении будет соответствовать новые возможности для эффективной организации и выпуска, а также CSS3 фрагменты кода для обеспечения совместимости с различными обозревателями.</span><span class="sxs-lookup"><span data-stu-id="e8f08-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="e8f08-142">Задача 1 - новые возможности редактора</span><span class="sxs-lookup"><span data-stu-id="e8f08-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="e8f08-143">В этой задаче будет обнаруживать новые функции в редакторе CSS.</span><span class="sxs-lookup"><span data-stu-id="e8f08-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="e8f08-144">Этот новый редактор поможет вам повысить свою производительность за счет использования новых отступа, в комментариях к коду, улучшенное и Расширенный список IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e8f08-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="e8f08-145">Запуск **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e8f08-146">В обозревателе решений откройте **Site.css** файла, расположенного в **стили** папки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="e8f08-147">Убедитесь, что **текстовый редактор** средства видны на панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="e8f08-148">Чтобы сделать это, выберите **представление** | **панелей инструментов** пункт меню и проверьте **текстовый редактор** параметры.</span><span class="sxs-lookup"><span data-stu-id="e8f08-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="e8f08-149">Можно будет заметить, что, начиная с этой новой версии, **комментарий** кнопки (![кнопки комментария](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) и **Uncomment** кнопки (![раскомментируйте кнопка](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) также включены для редактора CSS.</span><span class="sxs-lookup"><span data-stu-id="e8f08-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="e8f08-150">![Включение редактора и средств CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Включение редактора и средств CSS")</span><span class="sxs-lookup"><span data-stu-id="e8f08-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="e8f08-151">*Включение редактора и средств CSS*</span><span class="sxs-lookup"><span data-stu-id="e8f08-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="e8f08-152">Прокрутите код и выберите все определение класса CSS.</span><span class="sxs-lookup"><span data-stu-id="e8f08-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="e8f08-153">Нажмите кнопку **комментарий** (![кнопки комментария](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) кнопку, чтобы добавить комментарии для выбранных строк.</span><span class="sxs-lookup"><span data-stu-id="e8f08-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="e8f08-154">Щелкните **Uncomment** (![раскомментируйте кнопку](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) кнопку, чтобы отменить изменения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="e8f08-155">Нажмите кнопку **свернуть** (![свернуть](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) и **Expand** (![разверните](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) кнопок, расположенных в левом поле текста.</span><span class="sxs-lookup"><span data-stu-id="e8f08-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="e8f08-156">Обратите внимание на то, что теперь можно скрыть стили, которые вы не используете, чтобы получить более четкое представление.</span><span class="sxs-lookup"><span data-stu-id="e8f08-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="e8f08-157">![Свертывание классы CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "классы CSS, свертывание")</span><span class="sxs-lookup"><span data-stu-id="e8f08-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="e8f08-158">*Свертывание классы CSS*</span><span class="sxs-lookup"><span data-stu-id="e8f08-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="e8f08-159">Убедитесь, что параметр Автоматический отступ.</span><span class="sxs-lookup"><span data-stu-id="e8f08-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="e8f08-160">Выберите **средства** | **параметры** пункт меню, а затем выберите **текстовый редактор** | **CSS**  |  **Форматирование** страницы в левой части экрана.</span><span class="sxs-lookup"><span data-stu-id="e8f08-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="e8f08-161">Проверьте **иерархические отступы** параметр.</span><span class="sxs-lookup"><span data-stu-id="e8f08-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="e8f08-162">![Включение иерархические отступы](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Включение иерархические отступы")</span><span class="sxs-lookup"><span data-stu-id="e8f08-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="e8f08-163">*Включение иерархические отступы*</span><span class="sxs-lookup"><span data-stu-id="e8f08-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="e8f08-164">Найдите определение класса main (.main) и добавления элементов div стиля.</span><span class="sxs-lookup"><span data-stu-id="e8f08-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="e8f08-165">Обратите внимание, что код автоматически выравнивает помочь пользователям искать родительские классы с первого взгляда.</span><span class="sxs-lookup"><span data-stu-id="e8f08-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="e8f08-166">CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="e8f08-167">![Иерархические выравнивания в CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "иерархических выравнивания в CSS")</span><span class="sxs-lookup"><span data-stu-id="e8f08-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="e8f08-168">*Иерархические выравнивания в CSS*</span><span class="sxs-lookup"><span data-stu-id="e8f08-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="e8f08-169">Внутри **.main div** класса, найдите курсор в конце **границы: 0px;** и нажмите клавишу **ввод** для отображения списка IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e8f08-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="e8f08-170">Начните вводить **верхней** и обратите внимание на то, как список будет отфильтрован при вводе.</span><span class="sxs-lookup"><span data-stu-id="e8f08-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="e8f08-171">В списке будут отображаться элементы, которые содержат **верхней** в любой части слова (в предыдущих версиях Visual Studio, список фильтруется по элементам, *начать* с слова).</span><span class="sxs-lookup"><span data-stu-id="e8f08-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="e8f08-172">![Усовершенствования IntelliSense в CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "усовершенствования IntelliSense в CSS")</span><span class="sxs-lookup"><span data-stu-id="e8f08-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="e8f08-173">*Усовершенствования IntelliSense в CSS*</span><span class="sxs-lookup"><span data-stu-id="e8f08-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="e8f08-174">Задача 2 - средство выбора цвета</span><span class="sxs-lookup"><span data-stu-id="e8f08-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="e8f08-175">В этой задаче вы поймете, что новое средство выбора цвета CSS интегрированы в IntelliSense в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="e8f08-176">В **Site.css,** найдите определение класса заголовка (.header) и поместите курсор рядом с полем **цвет фона** атрибут между &quot;:&quot; и &quot; # &quot; символов в соответствующей строке кода **.**</span><span class="sxs-lookup"><span data-stu-id="e8f08-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="e8f08-177">![Поиск курсор](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "поиск курсора")</span><span class="sxs-lookup"><span data-stu-id="e8f08-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="e8f08-178">*Поиск курсора*</span><span class="sxs-lookup"><span data-stu-id="e8f08-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="e8f08-179">Удалить **двоеточие** (:) и записать его еще раз, чтобы отобразить средство выбора цвета.</span><span class="sxs-lookup"><span data-stu-id="e8f08-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="e8f08-180">Обратите внимание, что первый цвета, которые вы увидите наиболее частых цвета веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="e8f08-181">Если вы выберите белый цвет, его HTML-код (#fff) заменит текущий код цвета в таблице стилей.</span><span class="sxs-lookup"><span data-stu-id="e8f08-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="e8f08-182">![Палитра цветов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "палитра цветов")</span><span class="sxs-lookup"><span data-stu-id="e8f08-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="e8f08-183">*Палитра цветов*</span><span class="sxs-lookup"><span data-stu-id="e8f08-183">*Color picker*</span></span>
3. <span data-ttu-id="e8f08-184">Нажмите клавишу **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) кнопку в окне выбора цвета для отображения цветового градиента, а затем перетащите курсор градиента, чтобы выделить другим цветом.</span><span class="sxs-lookup"><span data-stu-id="e8f08-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="e8f08-185">После этого нажмите кнопку **Пипетка** кнопку и выбрать любой цвет на экране.</span><span class="sxs-lookup"><span data-stu-id="e8f08-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="e8f08-186">Обратите внимание на то, что значения цвета фона динамически изменяется при перемещении курсора.</span><span class="sxs-lookup"><span data-stu-id="e8f08-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="e8f08-187">![Средство выбора градиента](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "средство выбора градиента")</span><span class="sxs-lookup"><span data-stu-id="e8f08-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="e8f08-188">*Средство выбора градиента*</span><span class="sxs-lookup"><span data-stu-id="e8f08-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="e8f08-189">В **непрозрачности** ползунка, переход к относительно центральной части окна, чтобы уменьшить прозрачность.</span><span class="sxs-lookup"><span data-stu-id="e8f08-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="e8f08-190">Обратите внимание на то, что значение цвета фона теперь изменяется его масштаб для RGBA.</span><span class="sxs-lookup"><span data-stu-id="e8f08-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="e8f08-191">![Палитра цветов непрозрачности](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "палитра цветов непрозрачности")</span><span class="sxs-lookup"><span data-stu-id="e8f08-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="e8f08-192">*Палитра цветов непрозрачности*</span><span class="sxs-lookup"><span data-stu-id="e8f08-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-193">Определение цвета RGBA (красный, зеленый, синий, альфа-версия) в CSS3 позволяет определить значение непрозрачности цвета для одного элемента.</span><span class="sxs-lookup"><span data-stu-id="e8f08-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="e8f08-194">В отличие от **непрозрачности -** аналогичный атрибут CSS **-** RGBA цвета также совместимы с последних браузеров.</span><span class="sxs-lookup"><span data-stu-id="e8f08-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="e8f08-195">Задача 3 - фрагменты совместимого кода CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="e8f08-196">В этой задаче вы узнаете, как использовать межплатформенный совместимый CSS3 фрагменты для реализации некоторых функций веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="e8f08-197">В **Site.css** файл, найдите **заголовок** (.header) определение класса CSS и поместите курсор ниже **/ \*радиус границы\* /** заполнитель, чтобы добавить новый фрагмент.</span><span class="sxs-lookup"><span data-stu-id="e8f08-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="e8f08-198">Нажмите клавишу **ввод** для отображения в списке IntelliSense и введите **radius** для фильтрации списка.</span><span class="sxs-lookup"><span data-stu-id="e8f08-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="e8f08-199">Выберите **радиус границы** параметр из списка с помощью двойного щелчка, а затем нажмите клавишу **ВКЛАДКЕ** ключ для вставки фрагмента.</span><span class="sxs-lookup"><span data-stu-id="e8f08-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="e8f08-200">Введите размер radius в пикселях и нажать **ввод**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="e8f08-201">Например, введите **15px**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="e8f08-202">Атрибуты CSS3, добавленные во фрагменте будет отображаться границы со скругленными в большинстве браузеров соответствия HTML5, включая Mozilla и обозревателей на основе WebKit.</span><span class="sxs-lookup"><span data-stu-id="e8f08-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="e8f08-203">![Радиус границы фрагмента](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "радиус границы фрагмента")</span><span class="sxs-lookup"><span data-stu-id="e8f08-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="e8f08-204">*Радиус границы фрагмента*</span><span class="sxs-lookup"><span data-stu-id="e8f08-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="e8f08-205">Применить те же **границы** фрагменты кода в стиле страницы (.page).</span><span class="sxs-lookup"><span data-stu-id="e8f08-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="e8f08-206">CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="e8f08-207">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="e8f08-208">Обратите внимание на то, что каждая страница теперь будут скругленные границы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="e8f08-209">![Скругленные углы](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "скругленные углы")</span><span class="sxs-lookup"><span data-stu-id="e8f08-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="e8f08-210">*Скругленных углов*</span><span class="sxs-lookup"><span data-stu-id="e8f08-210">*Rounded corners*</span></span>
4. <span data-ttu-id="e8f08-211">Закройте браузер и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="e8f08-212">Откройте **Custom.css** файла, расположенного в **стили** папку, поместите курсор внутри **div.images ul li img** определение класса.</span><span class="sxs-lookup"><span data-stu-id="e8f08-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="e8f08-213">Нажмите клавишу ВВОД, чтобы отобразить список IntelliSense, тип **тень** и нажмите клавишу **ВКЛАДКЕ** ключ дважды, чтобы вставить фрагмент кода по умолчанию тени внутри определения класса.</span><span class="sxs-lookup"><span data-stu-id="e8f08-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="e8f08-214">Задайте значения тени **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="e8f08-215">Введите **радиус границы** и вставить фрагмент кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="e8f08-216">Тип **15px** задать размер radius и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="e8f08-217">![Скругленные углы с тенью](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "скругленные углы с тенью")</span><span class="sxs-lookup"><span data-stu-id="e8f08-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="e8f08-218">*Закругленные углы с тенью*</span><span class="sxs-lookup"><span data-stu-id="e8f08-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-219">В данный момент теневом атрибуте вставляется с соответствующий префикс (moz, webkit, o) для поддержки Mozilla и браузеры Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="e8f08-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="e8f08-220">Создайте новый класс **div.images ul li img:hover** ниже **div.images ul li img** определение класса, поместите курсор в угловых скобках **.**</span><span class="sxs-lookup"><span data-stu-id="e8f08-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="e8f08-221">CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="e8f08-222">Тип **преобразования** и нажмите клавишу **ВКЛАДКЕ** ключ дважды, чтобы вставить фрагмент кода преобразования.</span><span class="sxs-lookup"><span data-stu-id="e8f08-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="e8f08-223">Затем введите **rotate(-15deg)** для изменения значения угла поворота при образы при прохождении курсора.</span><span class="sxs-lookup"><span data-stu-id="e8f08-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="e8f08-224">CSS</span><span class="sxs-lookup"><span data-stu-id="e8f08-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="e8f08-225">Нажмите клавишу **F5** для запуска решения, а затем перейдите на страницу CSS3.</span><span class="sxs-lookup"><span data-stu-id="e8f08-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="e8f08-226">Обратите внимание на то, что образы имеют скругленные углы и поле теней.</span><span class="sxs-lookup"><span data-stu-id="e8f08-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="e8f08-227">Установите указатель мыши над изображениями, после чего они поворот.</span><span class="sxs-lookup"><span data-stu-id="e8f08-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="e8f08-228">![Преобразовать фрагмент кода, поворот изображения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "фрагмент преобразование, поворот изображения")</span><span class="sxs-lookup"><span data-stu-id="e8f08-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="e8f08-229">*Преобразовать фрагмент кода, поворот изображения*</span><span class="sxs-lookup"><span data-stu-id="e8f08-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-230">Если вы используете Internet Explorer 10 и не отображается теней, убедитесь, что режим документов настроена стандарты IE10.</span><span class="sxs-lookup"><span data-stu-id="e8f08-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="e8f08-231">Нажмите клавишу **F12** откройте средства разработчика Internet Explorer и нажмите кнопку **режим документов** менять стандарты IE10.</span><span class="sxs-lookup"><span data-stu-id="e8f08-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![о-США](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="e8f08-233">Упражнение 2: Новые возможности в редакторе HTML</span><span class="sxs-lookup"><span data-stu-id="e8f08-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="e8f08-234">Visual Studio содержит улучшенные редактора HTML.</span><span class="sxs-lookup"><span data-stu-id="e8f08-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="e8f08-235">Некоторые из усовершенствований, реализованных в этой версии, автоматический отступ в HTML-документы, фрагменты кода HTML5, HTML начала и окончания соответствие тегов и проверки HTML.</span><span class="sxs-lookup"><span data-stu-id="e8f08-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="e8f08-236">В рамках этого упражнения вы увидите, как эти изменения улучшить ваши степень при работе в разметке веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="e8f08-237">Как редактор CSS HTML-редактор также были внесены усовершенствования.</span><span class="sxs-lookup"><span data-stu-id="e8f08-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="e8f08-238">Большинство этих улучшений являются небольших, которые упрощают создание веб-разработчиков.</span><span class="sxs-lookup"><span data-stu-id="e8f08-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="e8f08-239">Отрадно дополнительные фрагменты кода для HTML5, автоматический отступ, сопоставления открывающий и закрывающий теги, когда изменения и проверки, предназначенные для DOCTYPE HTML-документа являются некоторые из этих усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="e8f08-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="e8f08-240">Задача 1 - улучшенные DOCTYPE проверки</span><span class="sxs-lookup"><span data-stu-id="e8f08-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="e8f08-241">В редакторе HTML теперь есть возможность проверить DOCTYPE страницы, несмотря на то, что определение может быть на главной странице.</span><span class="sxs-lookup"><span data-stu-id="e8f08-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="e8f08-242">В зависимости от типа документа, страницы редактор HTML будет проверять правильного набора правил и будет фильтровать список IntelliSense, учитывая элементы DOCTYPE.</span><span class="sxs-lookup"><span data-stu-id="e8f08-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="e8f08-243">В этой задаче мы изменим тип документа страницы, чтобы увидеть, как изменяется поведение редактора HTML.</span><span class="sxs-lookup"><span data-stu-id="e8f08-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="e8f08-244">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e8f08-245">Откройте **Site.Master** страницы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="e8f08-246">Обратите внимание, что в целевой схеме для проверки панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="e8f08-247">Способ работает редактор HTML (проверка, IntelliSense и т.д.) надлежащим образом изменится в соответствии с выбранной Doctype.</span><span class="sxs-lookup"><span data-stu-id="e8f08-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="e8f08-248">![Использовать тип документа в панели инструментов на редактирование HTML-кода](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype использования в панели инструментов на редактирование HTML-кода")</span><span class="sxs-lookup"><span data-stu-id="e8f08-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="e8f08-249">*Использовать тип документа в панели инструментов на редактирование HTML-кода*</span><span class="sxs-lookup"><span data-stu-id="e8f08-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="e8f08-250">Измените целевую схему для HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="e8f08-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="e8f08-251">![Изменение типа документа в панели инструментов на редактирование HTML-кода](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Doctype изменение в панели инструментов на редактирование HTML-кода")</span><span class="sxs-lookup"><span data-stu-id="e8f08-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="e8f08-252">*Изменение типа документа в панели инструментов на редактирование HTML-кода*</span><span class="sxs-lookup"><span data-stu-id="e8f08-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="e8f08-253">Поместите курсор под **текст** элемент и начните вводить имя элемента HTML5 (например, **видео**).</span><span class="sxs-lookup"><span data-stu-id="e8f08-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="e8f08-254">Обратите внимание на то, что элемент доступен не в списке IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e8f08-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="e8f08-255">![Элементы HTML5, не перечисленные](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "отсутствует в списке элементов HTML5")</span><span class="sxs-lookup"><span data-stu-id="e8f08-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="e8f08-256">*Отсутствует в списке элементов HTML5*</span><span class="sxs-lookup"><span data-stu-id="e8f08-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="e8f08-257">Отменить изменения в целевую схему для проверки инструментов комплектации DOCTYPE: XHTML5 из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="e8f08-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="e8f08-258">![Использовать тип документа в панели инструментов на редактирование HTML-кода](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype использования в панели инструментов на редактирование HTML-кода")</span><span class="sxs-lookup"><span data-stu-id="e8f08-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="e8f08-259">*Сброс Doctype в панели инструментов на редактирование HTML-кода*</span><span class="sxs-lookup"><span data-stu-id="e8f08-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="e8f08-260">Поместите курсор под **текст** элемент и начните вводить элемент HTML5 еще раз (например, такая как **видео**).</span><span class="sxs-lookup"><span data-stu-id="e8f08-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="e8f08-261">Обратите внимание на то, что элементы HTML5 теперь доступны в списке IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e8f08-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="e8f08-262">![Такие элементы HTML5 списке](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "указаны такие элементы HTML5")</span><span class="sxs-lookup"><span data-stu-id="e8f08-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="e8f08-263">*Перечислить элементы HTML5*</span><span class="sxs-lookup"><span data-stu-id="e8f08-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="e8f08-264">Задача 2 - начала и окончания теги автоматического обновления</span><span class="sxs-lookup"><span data-stu-id="e8f08-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="e8f08-265">Теперь Visual Studio обновляет HTML, открытие или закрытие теги элемента, который редактируется соответствуют друг другу.</span><span class="sxs-lookup"><span data-stu-id="e8f08-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="e8f08-266">Эта новая функция будет повысить эффективность работы, при редактировании HTML-теги.</span><span class="sxs-lookup"><span data-stu-id="e8f08-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="e8f08-267">На **Default.aspx** странице, добавьте **H3** элемента с заголовком (например, Visual Studio 2012 Rocks!).</span><span class="sxs-lookup"><span data-stu-id="e8f08-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="e8f08-268">Изменение **H3** тега и типа **H2** или **H1.**</span><span class="sxs-lookup"><span data-stu-id="e8f08-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="e8f08-269">Обратите внимание, что автоматически обновляет закрывающий тег.</span><span class="sxs-lookup"><span data-stu-id="e8f08-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="e8f08-270">Можно также изменить закрывающий тег, чтобы увидеть, что открывающий тег соответствующим образом обновляет слишком.</span><span class="sxs-lookup"><span data-stu-id="e8f08-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="e8f08-271">![Автоматическое обновление закрывающего тега](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "автоматическое обновление закрывающего тега")</span><span class="sxs-lookup"><span data-stu-id="e8f08-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="e8f08-272">*Автоматическое обновление закрывающего тега*</span><span class="sxs-lookup"><span data-stu-id="e8f08-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="e8f08-273">Задача 3 - новые фрагменты кода HTML5</span><span class="sxs-lookup"><span data-stu-id="e8f08-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="e8f08-274">Visual Studio теперь включает несколько фрагментов кода HTML5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="e8f08-275">В этой задаче будет использовать некоторые из этих фрагментов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="e8f08-276">Добавьте новую папку с именем **аудио** к корневой папке веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="e8f08-277">Откройте проводник Windows и скопируйте любой звуковой файл в **аудио** папке **WhatsNewASPNET.sln** решения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="e8f08-278">В **Default.aspx** найдите курсор под Web11 Rocks!</span><span class="sxs-lookup"><span data-stu-id="e8f08-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="e8f08-279">Заголовок.</span><span class="sxs-lookup"><span data-stu-id="e8f08-279">Header.</span></span> <span data-ttu-id="e8f08-280">Тип **аудио** и нажмите клавишу TAB.</span><span class="sxs-lookup"><span data-stu-id="e8f08-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="e8f08-281">Новый редактор HTML включает фрагменты кода для содержимого HTML5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="e8f08-282">Не забудьте использовать правильное определение типа документа, чтобы включить фрагменты кода HTML5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="e8f08-283">![Вставка фрагментов кода HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "вставки фрагментов кода HTML5")</span><span class="sxs-lookup"><span data-stu-id="e8f08-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="e8f08-284">*Вставка фрагментов кода HTML5*</span><span class="sxs-lookup"><span data-stu-id="e8f08-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="e8f08-285">Обновите источник звука, чтобы он указывал на существующий файл аудио.</span><span class="sxs-lookup"><span data-stu-id="e8f08-285">Update the audio source to point to an existing audio file.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="e8f08-286">Необходимо будет добавить звуковой файл в решение.</span><span class="sxs-lookup"><span data-stu-id="e8f08-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="e8f08-287">Нажмите клавишу **F5** запустите сайт и воспроизведения аудио.</span><span class="sxs-lookup"><span data-stu-id="e8f08-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="e8f08-288">![Запуск аудио элемента](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "под управлением элемент управления звуком")</span><span class="sxs-lookup"><span data-stu-id="e8f08-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="e8f08-289">*Под управлением элемент управления звуком*</span><span class="sxs-lookup"><span data-stu-id="e8f08-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-290">Можно также попытаться больше фрагментов кода, включенных в Visual Studio, таких как видео, рисунок и т. д.</span><span class="sxs-lookup"><span data-stu-id="e8f08-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="e8f08-291">Теперь попробуйте вставить элемент управления в определенной части страницы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="e8f08-292">Например, при попытке вставить **GridView** элемента управления, но вместо того чтобы вводить  **&lt;линий сетки,** начните вводить  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="e8f08-293">Обратите внимание, что в списке IntelliSense отображаются **asp: GridView** элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="e8f08-294">IntelliSense в редакторе HTML теперь предоставляет поиска регистр заголовков, а также частичного соответствия (получение все элементы, содержащие термин).</span><span class="sxs-lookup"><span data-stu-id="e8f08-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="e8f08-295">![Вставка элемента GridView с списки IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Вставка элемента GridView с списки IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="e8f08-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="e8f08-296">*Вставка элемента GridView с списки IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="e8f08-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="e8f08-297">При вводе  **&lt;сетки** вы получите все элементы, которые соответствуют запросу, но Visual Studio предложит **gridview** управления:</span><span class="sxs-lookup"><span data-stu-id="e8f08-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="e8f08-298">![Вставка элемента управления GridView с списки IntelliSense и частичное сопоставление](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Вставка элемента GridView с списки IntelliSense и частичное сопоставление")</span><span class="sxs-lookup"><span data-stu-id="e8f08-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="e8f08-299">*Вставка элемента управления GridView с списки IntelliSense и частичное сопоставление*</span><span class="sxs-lookup"><span data-stu-id="e8f08-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="e8f08-300">Задача 4 - редактор HTML смарт-тегов</span><span class="sxs-lookup"><span data-stu-id="e8f08-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="e8f08-301">Еще одно улучшение в редакторе HTML — это функция смарт-тегов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="e8f08-302">Смарт-теги упрощают выполнение задач разработки обычных или повторяющихся зависимости на уровне элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="e8f08-303">Эта функция уже была доступна в конструкторе HTML, но не в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="e8f08-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="e8f08-304">Откройте **Site.Master** и найдите **asp: меню** элемент.</span><span class="sxs-lookup"><span data-stu-id="e8f08-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="e8f08-305">Поместите курсор в открывающий тег и обратите внимание, что небольшой глифа, отображаемого в нижней части элемента - щелкните его, чтобы открыть меню смарт-задачи.</span><span class="sxs-lookup"><span data-stu-id="e8f08-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="e8f08-306">Обратите внимание на то, что имеется быстрый доступ для некоторых задач, связанных с элементом управления меню.</span><span class="sxs-lookup"><span data-stu-id="e8f08-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="e8f08-307">![Смарт-задачи для элемента управления меню](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "смарт-задачи для элемента управления меню")</span><span class="sxs-lookup"><span data-stu-id="e8f08-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="e8f08-308">*Смарт-задачи для элемента управления меню*</span><span class="sxs-lookup"><span data-stu-id="e8f08-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="e8f08-309">Задача 5 - автоматический отступ</span><span class="sxs-lookup"><span data-stu-id="e8f08-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="e8f08-310">Один из лучших решений в HTML отступов вложенных элементов, чтобы сохранить код для чтения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="e8f08-311">В Visual Studio 2012 можно заметить, что редактор автоматически смещает элементы, когда вы пишете код.</span><span class="sxs-lookup"><span data-stu-id="e8f08-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="e8f08-312">В предыдущей версии Visual Studio, автоматический отступ были доступны в редакторе XML, но не в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="e8f08-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="e8f08-313">Убедитесь, что конфигурация отступы в редакторе HTML присвоено автоматический отступ.</span><span class="sxs-lookup"><span data-stu-id="e8f08-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="e8f08-314">Чтобы сделать это, выберите **инструменты | Параметры** пункт меню, а затем выберите **текстовый редактор | HTML | Вкладки** страницы в левой части экрана.</span><span class="sxs-lookup"><span data-stu-id="e8f08-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="e8f08-315">Выберите параметр Автоматический отступ.</span><span class="sxs-lookup"><span data-stu-id="e8f08-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="e8f08-316">![Параметры редактора HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "параметры редактора HTML")</span><span class="sxs-lookup"><span data-stu-id="e8f08-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="e8f08-317">*Параметры редактора HTML*</span><span class="sxs-lookup"><span data-stu-id="e8f08-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="e8f08-318">На **Default.aspx** странице, удалите все содержимое в элементе аудио.</span><span class="sxs-lookup"><span data-stu-id="e8f08-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="e8f08-319">Поместите курсор в конце открывающий **аудио** элемент и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="e8f08-320">Обратите внимание, что новое положение курсора уровнем дополнительных отступа.</span><span class="sxs-lookup"><span data-stu-id="e8f08-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="e8f08-321">![Автоматический отступ в редакторе HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "автоматический отступ в редакторе HTML")</span><span class="sxs-lookup"><span data-stu-id="e8f08-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="e8f08-322">*Автоматический отступ в редакторе HTML*</span><span class="sxs-lookup"><span data-stu-id="e8f08-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="e8f08-323">Восстановление тег аудио с содержимым, удалены, или закройте **Default.aspx** без сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="e8f08-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="e8f08-324">Задача 6 - извлечение в пользовательский элемент управления</span><span class="sxs-lookup"><span data-stu-id="e8f08-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="e8f08-325">Средства рефакторинга, включается в Visual Studio, таких как извлечение части кода для функции — это отличные функции, упрощающие улучшение и рефакторингом исходного кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="e8f08-326">Какой для страниц ASP.NET будет извлекать HTML-код в пользовательский элемент управления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="e8f08-327">Делать это вручную будет включать в себя несколько действий, таких как создание нового пользовательского элемента управления, перемещение в разделе кода для пользовательского элемента управления, регистрации префикс тега для пользовательского элемента управления и, наконец, создание экземпляра пользовательского элемента управления на страницах.</span><span class="sxs-lookup"><span data-stu-id="e8f08-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="e8f08-328">Теперь новый *извлечь в пользовательский элемент управления* средство автоматически выполняет эти действия для вас.</span><span class="sxs-lookup"><span data-stu-id="e8f08-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="e8f08-329">В этой задаче будет использовать новый извлечение в пользовательский элемент управления контекстные операции, чтобы создать новый пользовательский элемент управления на основе выбранного кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="e8f08-330">На **Default.aspx** выберите **H2** и **аудио** элементов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="e8f08-331">Щелкните правой кнопкой мыши и выберите **извлечь в пользовательский элемент управления**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="e8f08-332">![Извлечь в пользовательский элемент управления меню параметр](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "извлечь в пользовательский элемент управления меню")</span><span class="sxs-lookup"><span data-stu-id="e8f08-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="e8f08-333">*Извлечь в пользовательский элемент управления меню*</span><span class="sxs-lookup"><span data-stu-id="e8f08-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="e8f08-334">Введите имя для нового пользовательского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-334">Type a name for the new user control.</span></span> <span data-ttu-id="e8f08-335">Например **Jukebox.ascx**, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="e8f08-336">![Сохранение извлеченные пользовательский элемент управления](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "сохранения извлеченных пользовательского элемента управления")</span><span class="sxs-lookup"><span data-stu-id="e8f08-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="e8f08-337">*Сохранение извлеченные пользовательского элемента управления*</span><span class="sxs-lookup"><span data-stu-id="e8f08-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="e8f08-338">Обратите внимание на то, что выделенный код были извлечены в пользовательский элемент управления, и исходное расположение выбранного кода было заменено значением экземпляра нового пользовательского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="e8f08-339">![Страницы автоматически обновляются для использования нового пользовательского элемента управления](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "страницы автоматически обновляются для использования нового пользовательского элемента управления")</span><span class="sxs-lookup"><span data-stu-id="e8f08-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="e8f08-340">*Страницы автоматически обновляются для использования нового пользовательского элемента управления*</span><span class="sxs-lookup"><span data-stu-id="e8f08-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="e8f08-341">Нажмите клавишу **F5** запустить страницу и убедитесь, что он работает.</span><span class="sxs-lookup"><span data-stu-id="e8f08-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="e8f08-342">Упражнение 3: Новые возможности в редакторе JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8f08-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="e8f08-343">Написания и редактирования кода JavaScript не является непростой задачей, особенно в том случае, при запуске приложения к увеличению размера, и вы обнаружите освободили много файлов и сотни функций.</span><span class="sxs-lookup"><span data-stu-id="e8f08-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="e8f08-344">Скрипт разработчикам обычно приходится выполнить некоторые дополнительные действия для поддержания читаемость кода и переходить между файлами.</span><span class="sxs-lookup"><span data-stu-id="e8f08-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="e8f08-345">С включением библиотек JavaScript, таких как jQuery скрипт навигации стала самой сложной задачей из-за длина кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="e8f08-346">Редактор JavaScript с объектом promise, чтобы сделать код режима доступны и организованных обновил Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="e8f08-347">Многие функции Visual Studio, которые уже существовали в C# или VB редакторах теперь реализованы в редакторе JavaScript: перейти к определению, автоматический отступ, документацию и проверки при написании.</span><span class="sxs-lookup"><span data-stu-id="e8f08-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="e8f08-348">С помощью обновленного списка IntelliSense имеется каталог функции JavaScript в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="e8f08-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="e8f08-349">В этом упражнении вы узнаете некоторые новые функции и усовершенствования редактора JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="e8f08-350">Будет просматривать файлы образцов и обнаружение всех новые параметры, которые облегчат программирования JavaScript в Visual Studio 2012 эффективнее.</span><span class="sxs-lookup"><span data-stu-id="e8f08-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="e8f08-351">Задача 1 - новые функции редактора JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8f08-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="e8f08-352">Эта задача будет представлена некоторые новые возможности редактора JavaScript, которые нацелены на размещение кода и поставке удобства работы пользователя.</span><span class="sxs-lookup"><span data-stu-id="e8f08-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="e8f08-353">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e8f08-354">Нажмите клавишу **F5** для запуска приложения, щелкните ссылку "JavaScript" на панели навигации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="e8f08-355">Обновите страницу несколько раз и проверьте как увеличения значения счетчика.</span><span class="sxs-lookup"><span data-stu-id="e8f08-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="e8f08-356">![Счетчика страниц](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "счетчика страниц")</span><span class="sxs-lookup"><span data-stu-id="e8f08-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="e8f08-357">*Счетчика страниц*</span><span class="sxs-lookup"><span data-stu-id="e8f08-357">*Page counter*</span></span>
3. <span data-ttu-id="e8f08-358">Закройте браузер и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="e8f08-359">Откройте **JavaScript.aspx** страницы и найдите **&lt;сценарий&gt;** блока (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="e8f08-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="e8f08-360">В следующем коде используется локальное хранилище HTML5 для хранения *pageLoadCount* переменной, которая хранит количество посещений страницы для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="e8f08-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="e8f08-361">Локальное хранилище — это база данных ключ значение на стороне клиента, представленных в стандарте HTML5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="e8f08-362">Данные сохраняются на локальном компьютере, в браузере пользователя.</span><span class="sxs-lookup"><span data-stu-id="e8f08-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="e8f08-363">Убедитесь, что DOCTYPE присваивается XHTML5 приступать к выполнению дальнейших действий.</span><span class="sxs-lookup"><span data-stu-id="e8f08-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="e8f08-364">Изменить код, обратите внимание на то, что IntelliSense для JavaScript включает функции HTML5, такие как локальное хранилище и их внутренними методами.</span><span class="sxs-lookup"><span data-stu-id="e8f08-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="e8f08-365">![Функции HTML5 JavaScript JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript функции JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e8f08-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="e8f08-366">*Функции HTML5 JavaScript в JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="e8f08-367">Щелкните любой квадратную скобку (**{**) из сценария, кода и обратите внимание на то, что скобки выделяются.</span><span class="sxs-lookup"><span data-stu-id="e8f08-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="e8f08-368">![Квадратные скобки, выделяются](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "квадратные скобки, выделяются.")</span><span class="sxs-lookup"><span data-stu-id="e8f08-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="e8f08-369">*Квадратные скобки, выделяются.*</span><span class="sxs-lookup"><span data-stu-id="e8f08-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="e8f08-370">Раскомментируйте функция **testAutoAlign()** (выберите три строки и воспользуйтесь **CTRL** + **K**; **CTRL** + **U**) и найдите курсора в коде функции.</span><span class="sxs-lookup"><span data-stu-id="e8f08-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="e8f08-371">Нажмите клавишу ВВОД для добавления второй строки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-371">Press enter to append a second line.</span></span> <span data-ttu-id="e8f08-372">Обратите внимание, что код будет выглядеть **выровнены** и **автоматическим отступом**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="e8f08-373">![Код JavaScript будет автоматически выровнен](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "код JavaScript будет автоматически выровнен")</span><span class="sxs-lookup"><span data-stu-id="e8f08-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="e8f08-374">*Код JavaScript будет автоматически выровнен*</span><span class="sxs-lookup"><span data-stu-id="e8f08-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="e8f08-375">Задача 2 - Проверка JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8f08-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="e8f08-376">В этой задаче будет обнаруживать новые проверки JavaScript для стандартных ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="e8f08-377">Эта функция поможет вам методику написания совместимого кода JavaScript, при Предотвращение сценарных перед развертыванием сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="e8f08-378">Visual Studio 2010 реализованы ECMAStript3 соответствия, хотя Visual Studio 2012 обеспечивает соответствие ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="e8f08-379">Откройте **ECMA5script5.js** каталоге **Scripts\custom** папки проекта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="e8f08-380">Теперь можно протестировать проверку для стандартных ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="e8f08-381">Вы можете проверить &quot; **используйте strict** &quot; направление, в первой строке файла, который позволяет ECMAScript5 **строгий режим**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="e8f08-382">В этом режиме состоит в подмножестве языка, поясняющий неоднозначности последний выпуск и добавляет некоторые новые возможности, такие как методы получения и задания, поддержка библиотек для JSON и более полный отражения на свойства объекта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="e8f08-383">Откройте **список ошибок** Если еще не открыт (**представление** меню | **Список ошибок**).</span><span class="sxs-lookup"><span data-stu-id="e8f08-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="e8f08-384">Обратите внимание, что **функция** подчеркнуты объявления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="e8f08-385">Это обусловлено в ECMA5 стандартные функции не могут располагаться внутри структуры языка.</span><span class="sxs-lookup"><span data-stu-id="e8f08-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="e8f08-386">В ошибке списка ниже вы увидите сведения о предупреждении.</span><span class="sxs-lookup"><span data-stu-id="e8f08-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="e8f08-387">![Сообщение об ошибке проверки JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "сообщение об ошибке проверки JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e8f08-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="e8f08-388">*Сообщение об ошибке проверки JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="e8f08-389">Закомментируйте **&quot;используйте strict&quot;** направление и обратите внимание, что ошибки исчезают, но остаются предупреждения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="e8f08-390">В последней строке файла, запись любую строку как **&quot;тестирования&quot;** (заключать в кавычки, чтобы указать это в виде строки).</span><span class="sxs-lookup"><span data-stu-id="e8f08-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="e8f08-391">Записи периода рядом со строкой для отображения списка IntelliSense и выбора **trim** параметр.</span><span class="sxs-lookup"><span data-stu-id="e8f08-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="e8f08-392">В standard ECMAScript5 строковые значения и переменные имеют методы строк, определенные как обрезать, верхний регистр, поиска и замены.</span><span class="sxs-lookup"><span data-stu-id="e8f08-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="e8f08-393">![Список IntelliSense в JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "список IntelliSense в JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e8f08-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="e8f08-394">*Список IntelliSense в JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="e8f08-395">Задача 3 - XML-документации для JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8f08-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="e8f08-396">В этой задаче будет изучено функций Visual Studio для XML-документации в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="e8f08-397">Вы увидите, что в списке IntelliSense для JavaScript теперь отображаются в XML-документации каждой функции.</span><span class="sxs-lookup"><span data-stu-id="e8f08-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="e8f08-398">Кроме того вы поймете, функцию навигации в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="e8f08-399">Откройте **XMLDoc.js** файл, расположенный в **скрипты или настраиваемый** папки проекта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="e8f08-400">Этот файл содержит XML-документации на каждой из функций JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="e8f08-401">![Документация JavaScript XML интегрированы в IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML-документации, интегрированы в IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="e8f08-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="e8f08-402">*Документация XML в JavaScript, интегрированы в IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="e8f08-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="e8f08-403">Ниже **добавить** работать в **XMLDoc.js** создайте новую функцию с именем **тестирования**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="e8f08-404">В **тестирования** вызовите метод **умножение** функцию, которая получает два параметра.</span><span class="sxs-lookup"><span data-stu-id="e8f08-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="e8f08-405">Обратите внимание на то, отображается окно подсказки **умножение** документации по функции.</span><span class="sxs-lookup"><span data-stu-id="e8f08-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="e8f08-406">![XML-документации для функций JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML-документации для функций JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e8f08-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="e8f08-407">*XML-документации для функций JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="e8f08-408">Выполните инструкцию вызова функции и тип *точка* для открытия списка IntelliSense на возвращаемое значение.</span><span class="sxs-lookup"><span data-stu-id="e8f08-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="e8f08-409">Обратите внимание, что Visual Studio обнаруживает **возвращают** значение в документации, рассматривая значение в виде числа.</span><span class="sxs-lookup"><span data-stu-id="e8f08-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="e8f08-410">![XML-документацию для возвращаемых типов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML-документацию для возвращаемых типов")</span><span class="sxs-lookup"><span data-stu-id="e8f08-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="e8f08-411">*XML-документацию для возвращаемых типов*</span><span class="sxs-lookup"><span data-stu-id="e8f08-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="e8f08-412">Теперь вставьте вызов, чтобы добавить функцию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-412">Now, insert a call to add function.</span></span> <span data-ttu-id="e8f08-413">Обратите внимание, что редактор JavaScript теперь поддерживает перегрузки функций.</span><span class="sxs-lookup"><span data-stu-id="e8f08-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="e8f08-414">При написании имени функции, можно выбрать любой из доступных перегрузок, указанным в документации по.</span><span class="sxs-lookup"><span data-stu-id="e8f08-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="e8f08-415">![XML-документации для перегрузок](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML-документации для перегрузок")</span><span class="sxs-lookup"><span data-stu-id="e8f08-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="e8f08-416">*XML-документации для перегрузок*</span><span class="sxs-lookup"><span data-stu-id="e8f08-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="e8f08-417">Откройте **GotoDefinition.js** файл и найдите **$().html()** вызов функции.</span><span class="sxs-lookup"><span data-stu-id="e8f08-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="e8f08-418">Найти курсор на **html**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="e8f08-419">Нажмите клавишу **F12** и перейти к определению.</span><span class="sxs-lookup"><span data-stu-id="e8f08-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="e8f08-420">Обратите внимание, что теперь можно получить доступ к и просмотр кода JavaScript без использования **найти** средство.</span><span class="sxs-lookup"><span data-stu-id="e8f08-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="e8f08-421">Найдите курсор на инструкцию jQuery до блок подписи в нижней части файла кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="e8f08-422">Нажмите клавишу **F12**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-422">Press **F12**.</span></span> <span data-ttu-id="e8f08-423">Произойдет переход к файлу библиотеки jQuery.</span><span class="sxs-lookup"><span data-stu-id="e8f08-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="e8f08-424">Обратите внимание, что можно также перейти в файлах jQuery, с помощью **F12**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="e8f08-425">![Переход к определениям jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "перехода к определениям jQuery")</span><span class="sxs-lookup"><span data-stu-id="e8f08-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="e8f08-426">*Переход к определениям jQuery*</span><span class="sxs-lookup"><span data-stu-id="e8f08-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="e8f08-427">Убедитесь, что GotoDefinition.js имеет синтаксические ошибки перед сохранением файла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="e8f08-428">Упражнение 4: Объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="e8f08-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="e8f08-429">Сколько раз следует веб-сайты включать более одного JavaScript или CSS-файл?</span><span class="sxs-lookup"><span data-stu-id="e8f08-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="e8f08-430">Это очень распространенный сценарий, где объединения и минификации может помочь уменьшить размер файла и сделать узел работать быстрее.</span><span class="sxs-lookup"><span data-stu-id="e8f08-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="e8f08-431">Новая функция объединения в ASP.NET 4.5 упаковывает набор файлов, JS и CSS в один элемент и уменьшает его размер, Минификация встроенного содержимого (т. е. Удаление не требуется пробелов, удаление комментариев, уменьшая идентификаторов).</span><span class="sxs-lookup"><span data-stu-id="e8f08-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="e8f08-432">Объединение и Минификация в ASP.NET 4.5 выполняется во время выполнения, чтобы процесс идентификации агента пользователя (например IE, Mozilla и т. д) и таким образом, улучшить сжатие, обозревателя пользователя (например, удаление сильно удаленных объектов Mozilla конкретных Если запрос поступает от IE).</span><span class="sxs-lookup"><span data-stu-id="e8f08-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="e8f08-433">В этом упражнении вы узнаете, как включить и использовать различные виды объединение и Минификация в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e8f08-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="e8f08-434">Задача 1 - Установка в объединение и Минификация пакет из NuGet</span><span class="sxs-lookup"><span data-stu-id="e8f08-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="e8f08-435">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e8f08-436">Откройте консоль диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="e8f08-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="e8f08-437">Чтобы сделать это, используйте меню **представление** | **Other Windows** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="e8f08-438">![Открытие file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole диспетчера пакета](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Открытие консоли диспетчера пакетов")</span><span class="sxs-lookup"><span data-stu-id="e8f08-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="e8f08-439">*Открытие консоли диспетчера пакетов*</span><span class="sxs-lookup"><span data-stu-id="e8f08-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="e8f08-440">В **консоль диспетчера пакетов** тип **Microsoft.Web.Optimization Install-Package** и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="e8f08-441">Задача 2 - пакеты по умолчанию</span><span class="sxs-lookup"><span data-stu-id="e8f08-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="e8f08-442">Самый простой способ использования объединения и минификации является для включения пакетов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="e8f08-443">Этот метод использует соглашения можно ссылаться на объединенные в пакет и минифицированные версию для файлов в папке JS и CSS.</span><span class="sxs-lookup"><span data-stu-id="e8f08-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="e8f08-444">В этой задаче вы узнаете, как включить и ссылаться на объединенные в пакет и минифицированные файлы JS и CSS и просмотрите результаты.</span><span class="sxs-lookup"><span data-stu-id="e8f08-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="e8f08-445">Если еще не открыт, запустите **Visual Studio** и откройте **WhatsNewASPNET.sln** решение находится в **Source\WhatsNewASPNET** папку данную лабораторию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e8f08-446">В **обозревателе решений**, разверните **стили**, **Scripts\custom** и **Scripts\bundle** папки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="e8f08-447">Обратите внимание на то, что приложение использует более одного CSS и JS-файл.</span><span class="sxs-lookup"><span data-stu-id="e8f08-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="e8f08-448">![Файлы несколько таблиц стилей и JavaScript в приложении](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "файлов несколько таблиц стилей и JavaScript в приложении")</span><span class="sxs-lookup"><span data-stu-id="e8f08-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="e8f08-449">*Несколько файлов таблицы стилей и JavaScript в приложении*</span><span class="sxs-lookup"><span data-stu-id="e8f08-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="e8f08-450">Откройте **Global.asax.cs** файл.</span><span class="sxs-lookup"><span data-stu-id="e8f08-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="e8f08-451">Обратите внимание, что новый **Microsoft.Web.Optimization** пространство имен закомментирован в начале файла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="e8f08-452">Удалите комментарий с помощью директивы для включения функции объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-452">Uncomment the using directive to include the bundling and minification features.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="e8f08-453">Найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="e8f08-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="e8f08-454">В этом методе раскомментировав вызов EnableDefaultBundles, как показано в приведенном ниже фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="e8f08-455">Это позволяет нам ссылки на распространение коллекцию CSS-файлов в папке, используя путь к этой папке, а также &quot;CSS&quot; или &quot;JS&quot; суффикс.</span><span class="sxs-lookup"><span data-stu-id="e8f08-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="e8f08-456">Откройте **Optimization.aspx** файл и найдите элемент управления содержимым для **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="e8f08-457">Обратите внимание, что файлы CSS и JS с тегом единый на которую указывает ссылка.</span><span class="sxs-lookup"><span data-stu-id="e8f08-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="e8f08-458">Этот код предназначен для демонстрационных целей.</span><span class="sxs-lookup"><span data-stu-id="e8f08-458">This code is for demo purposes.</span></span> <span data-ttu-id="e8f08-459">В идеале будет ссылаться на пакеты в файл Site.Master.</span><span class="sxs-lookup"><span data-stu-id="e8f08-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="e8f08-460">В этом примере кода вы найдете что некоторые связанным файлам также ссылок на файл Site.Master, что делает это последняя ссылка избыточное.</span><span class="sxs-lookup"><span data-stu-id="e8f08-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="e8f08-461">Обратите внимание на то, что ссылки с помощью объединения правил, описанных в **href** атрибут для получения всех CSS или JS-файлов из стили и Scripts\custom папки соответственно.</span><span class="sxs-lookup"><span data-stu-id="e8f08-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="e8f08-462">Можно использовать путь **сценарии/пользовательские/JS** как показано ниже, чтобы объединение и Минификация файлы JS внутри **скрипты или настраиваемый** папки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="e8f08-463">Это поведение по умолчанию с помощью пакетов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-463">This is the default behavior with the default bundles.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="e8f08-464">Откройте **Styles\Site.css** файла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="e8f08-465">Обратите внимание на то, что в исходный CSS-файл содержит структурированный код, пробелы и комментарии, увеличить размер файла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="e8f08-466">(Также файл JavaScript содержит пробелы и комментарии).</span><span class="sxs-lookup"><span data-stu-id="e8f08-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="e8f08-467">![Один из исходного CSS файлов в папке Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "одного из исходного CSS файлов в папке Scripts")</span><span class="sxs-lookup"><span data-stu-id="e8f08-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="e8f08-468">*Один из исходных файлов CSS в папку «скрипты»*</span><span class="sxs-lookup"><span data-stu-id="e8f08-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="e8f08-469">Нажмите клавишу **F5** запустите приложение и перейдите к **оптимизации** страницы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="e8f08-470">Щелкните **пакета CSS** ссылку, чтобы скачать и открыть файл.</span><span class="sxs-lookup"><span data-stu-id="e8f08-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="e8f08-471">Ознакомьтесь с минифицированные объединенный файл.</span><span class="sxs-lookup"><span data-stu-id="e8f08-471">Check out the minified bundled file.</span></span> <span data-ttu-id="e8f08-472">Можно заметить, что были удалены все пробелы, комментарии и символы отступа, созданию файла меньшего размера.</span><span class="sxs-lookup"><span data-stu-id="e8f08-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="e8f08-473">![Объединенными файлами CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "объединенными CSS-файлов")</span><span class="sxs-lookup"><span data-stu-id="e8f08-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="e8f08-474">*Распространение файлов CSS*</span><span class="sxs-lookup"><span data-stu-id="e8f08-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="e8f08-475">Теперь щелкните **пакета JS** ссылку, чтобы открыть файл входит в JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="e8f08-476">Можно без опасений пренебречь обозревателя предупреждение.</span><span class="sxs-lookup"><span data-stu-id="e8f08-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="e8f08-477">Обратите внимание, что файлы JavaScript в папке **пользовательских** папки также входит и уменьшено.</span><span class="sxs-lookup"><span data-stu-id="e8f08-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="e8f08-478">![Объединенными файлами JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "файлы JavaScript, объединенными")</span><span class="sxs-lookup"><span data-stu-id="e8f08-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="e8f08-479">*Пакетные файлы JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="e8f08-480">Включение сжатия для файлов CSS и JS оказался гораздо более сложным в предыдущей версии ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e8f08-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="e8f08-481">Теперь, как вы уже видели, необходимо просто добавить одну строку в *Global.asax* нужно включить объединение и затем ссылаться на пакетные файлы с веб-узла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="e8f08-482">Задача 3 - статические пакеты</span><span class="sxs-lookup"><span data-stu-id="e8f08-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="e8f08-483">Статический набор подход позволяет настраивать набор файлов для пакета, ссылки и Минификация метод, который будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="e8f08-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="e8f08-484">В этой задаче вы настроите статический набор, чтобы определить определенный набор файлов для объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="e8f08-485">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="e8f08-485">Close the browser.</span></span>
2. <span data-ttu-id="e8f08-486">Откройте **Global.asax.cs** файл и найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="e8f08-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="e8f08-487">Раскомментируйте код статический пакета, как показано в приведенном ниже коде.</span><span class="sxs-lookup"><span data-stu-id="e8f08-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="e8f08-488">При определении статического пакет, который будет ссылаться с помощью &quot; **~/StaticBundle** &quot; виртуальный путь и используйте **JsMinify** для Минификация всех указанных файлов с помощью **AddFile** метод.</span><span class="sxs-lookup"><span data-stu-id="e8f08-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="e8f08-489">Наконец, вы добавляете статический набор, чтобы **BundleTable** и его включение.</span><span class="sxs-lookup"><span data-stu-id="e8f08-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="e8f08-490">Обратите внимание на то, что файлы не находятся в одном месте; Это еще одно преимущество перед объединением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e8f08-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="e8f08-491">Откройте **Optimization.aspx** файла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="e8f08-492">Обратите внимание, что ссылку, чтобы **статических JS пакета** используется путь, объявленные при настройке статических пакета в файле Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="e8f08-493">Нажмите клавишу **F5** для запуска приложения, а затем перейдите к **оптимизации** страницы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="e8f08-494">Щелкните **статических JS пакета** ссылку, чтобы открыть файл.</span><span class="sxs-lookup"><span data-stu-id="e8f08-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="e8f08-495">Обратите внимание на то, что минифицированные входит файл JavaScript — это выходные данные для всех файлов JavaScript, настраивается в файле статического пакета по пути &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="e8f08-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="e8f08-496">![Статический набор файлов JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "JavaScript статические файлы пакета")</span><span class="sxs-lookup"><span data-stu-id="e8f08-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="e8f08-497">*Статический набор файлов JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="e8f08-498">Закройте браузер и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="e8f08-499">Задача 4 - папку динамических пакетов</span><span class="sxs-lookup"><span data-stu-id="e8f08-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="e8f08-500">В этой задаче вы узнаете, как настроить динамический папки пакетов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="e8f08-501">Возможности динамического объединения — что можно включить статический JavaScript, а также другие файлы на языках, компилируемый в JavaScript и таким образом, требуют некоторых действий, объединение которого выполняется.</span><span class="sxs-lookup"><span data-stu-id="e8f08-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="e8f08-502">В этом примере вы узнаете, как использовать **DynamicFolderBundle** класса для создания динамического пакета для файлов, записанных в CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="e8f08-503">CofeeScript — это язык программирования, который компилируется в JavaScript и предоставляет более простой синтаксис для написания кода JavaScript, повышая краткости JavaScript и удобства чтения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="e8f08-504">Откройте **Global.asax.cs** файл и найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="e8f08-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="e8f08-505">Раскомментируйте код динамического пакета, как показано в приведенном ниже коде.</span><span class="sxs-lookup"><span data-stu-id="e8f08-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="e8f08-506">При определении пакета динамического папки, в которую будет использовать **CoffeeMinify** пользовательских минификации процессором, который будет применяться только к файлам с &quot; **.coffee** &quot; (расширение Файлы CoffeeScript).</span><span class="sxs-lookup"><span data-stu-id="e8f08-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="e8f08-507">Обратите внимание на то, что можно использовать шаблон поиска для выбора файлов для формирования пакета в папке, например "\*.coffee".</span><span class="sxs-lookup"><span data-stu-id="e8f08-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="e8f08-508">Откройте консоль диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="e8f08-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="e8f08-509">Чтобы сделать это, используйте меню **представление** | **Other Windows** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="e8f08-510">В **консоль диспетчера пакетов** тип **CoffeeSharp Install-Package** и нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="e8f08-511">Нажмите кнопку **Показать все файлы** кнопку **обозревателе решений** окно</span><span class="sxs-lookup"><span data-stu-id="e8f08-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="e8f08-512">![Отображение всех файлов](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "отображение всех файлов")</span><span class="sxs-lookup"><span data-stu-id="e8f08-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="e8f08-513">*Отображение всех файлов*</span><span class="sxs-lookup"><span data-stu-id="e8f08-513">*Showing all files*</span></span>
6. <span data-ttu-id="e8f08-514">Щелкните правой кнопкой мыши **CoffeeMinify.cs** файл **обозревателе решений** и выберите **включить в проект**</span><span class="sxs-lookup"><span data-stu-id="e8f08-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="e8f08-515">![Включить в проект файл CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "включить CoffeeMinify.cs файл в проект")</span><span class="sxs-lookup"><span data-stu-id="e8f08-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="e8f08-516">*Включить CoffeeMinify.cs файл в проект*</span><span class="sxs-lookup"><span data-stu-id="e8f08-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="e8f08-517">Откройте **CoffeeMinify.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="e8f08-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="e8f08-518">Этот класс наследует от JsMinify для уменьшения размера выходных данных JavaScript, полученный в результате компиляции кода CoffeeScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="e8f08-519">Он вызывает CoffeeScript компилятору создавать код JavaScript, сначала, а затем он отправляет его методу JsMinify.Process для уменьшения размера полученного кода.</span><span class="sxs-lookup"><span data-stu-id="e8f08-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="e8f08-520">Откройте **Script1.coffee** и **Script2.coffee** файлов из **скриптов или пакета** папки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="e8f08-521">Эти файлы содержат код CoffeScript компилируется во время выполнения объединения с классом CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="e8f08-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="e8f08-522">В целях упрощения предоставленных файлах CoffeeScript только включая примеры кода и CoffeeScript.</span><span class="sxs-lookup"><span data-stu-id="e8f08-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="e8f08-523">Комментарии не распространяется процессом JsMinify.</span><span class="sxs-lookup"><span data-stu-id="e8f08-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="e8f08-524">![Файлы CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "файлы CoffeeScript")</span><span class="sxs-lookup"><span data-stu-id="e8f08-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="e8f08-525">*Файлы CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="e8f08-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) предоставляет более простой синтаксис для написания кода JavaScript, повышая краткости JavaScript и удобства чтения, а также добавление других функций, таких как понимание массива и сопоставление шаблонов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="e8f08-527">Откройте **Optimization.aspx** файл и найдите ссылки пакета.</span><span class="sxs-lookup"><span data-stu-id="e8f08-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="e8f08-528">Обратите внимание, что ссылку, чтобы **динамического пакета JS** ссылается на **скриптов или пакета** папки с помощью **/кофе** настроен для пакета папки динамического суффикс.</span><span class="sxs-lookup"><span data-stu-id="e8f08-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="e8f08-529">Нажмите клавишу **F5** для запуска приложения, а затем перейдите к **оптимизации** страницы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="e8f08-530">Щелкните **динамического пакета JS** ссылку, чтобы открыть созданный файл.</span><span class="sxs-lookup"><span data-stu-id="e8f08-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="e8f08-531">Обратите внимание, что содержимое, которое было включено в этот пакет содержит только **.coffee** файлов.</span><span class="sxs-lookup"><span data-stu-id="e8f08-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="e8f08-532">Также вы увидите, что CoffeeScript код был скомпилирован в код JavaScript и закомментированных строк был удален.</span><span class="sxs-lookup"><span data-stu-id="e8f08-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="e8f08-533">![Динамические файлы JS объединить](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "динамического JS файлы пакета")</span><span class="sxs-lookup"><span data-stu-id="e8f08-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="e8f08-534">*Объединить динамических JS-файлов*</span><span class="sxs-lookup"><span data-stu-id="e8f08-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="e8f08-535">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложении B: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="e8f08-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e8f08-536">Сводка</span><span class="sxs-lookup"><span data-stu-id="e8f08-536">Summary</span></span>

<span data-ttu-id="e8f08-537">Это лабораторное занятие поможет вам понять, что такое возможности ASP.NET и веб-разработки в Visual Studio 2012 и как воспользоваться преимуществами ряда усовершенствований в Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="e8f08-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="e8f08-538">Выполнив этот практический семинар, выученных, как использовать новые функции и усовершенствования в Visual Studio 2012 редакторы CSS, JavaScript и HTML.</span><span class="sxs-lookup"><span data-stu-id="e8f08-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="e8f08-539">Кроме того выученных, как в Visual Studio 2012 реализованы встроенные объединения и минификации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e8f08-540">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="e8f08-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e8f08-541">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e8f08-542">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="e8f08-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e8f08-543">Перейдите к [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e8f08-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e8f08-544">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e8f08-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e8f08-545">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-545">Click on **Install Now**.</span></span> <span data-ttu-id="e8f08-546">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="e8f08-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e8f08-547">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e8f08-548">![Установка Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e8f08-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e8f08-549">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e8f08-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e8f08-550">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="e8f08-552">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="e8f08-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e8f08-553">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-553">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="e8f08-555">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="e8f08-555">*Installation progress*</span></span>
6. <span data-ttu-id="e8f08-556">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-556">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="e8f08-558">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="e8f08-558">*Installation completed*</span></span>
7. <span data-ttu-id="e8f08-559">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="e8f08-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e8f08-560">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="e8f08-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="e8f08-562">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="e8f08-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e8f08-563">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="e8f08-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="e8f08-564">В этом приложении показано, как создать новый веб-сайт на портале управления Windows Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f08-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="e8f08-565">Задача 1 - Создание нового веб-сайта с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e8f08-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="e8f08-566">Перейдите к [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="e8f08-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-567">С помощью Windows Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика.</span><span class="sxs-lookup"><span data-stu-id="e8f08-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="e8f08-568">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="e8f08-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="e8f08-569">![Войдите на портал Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Войдите на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e8f08-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="e8f08-570">*Войдите на портал управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="e8f08-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="e8f08-571">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="e8f08-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="e8f08-572">![Создание нового веб-сайта](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="e8f08-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="e8f08-573">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="e8f08-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="e8f08-574">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="e8f08-575">Затем выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="e8f08-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="e8f08-576">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-577">Веб-сайта Windows Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="e8f08-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="e8f08-578">Быстрое создание позволяет развернуть завершенное веб-приложения для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="e8f08-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="e8f08-579">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="e8f08-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="e8f08-580">![Создание нового веб-сайта, с помощью быстрого создания](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "создания нового веб-сайта, с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="e8f08-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="e8f08-581">*Создание нового веб-сайта, с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="e8f08-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="e8f08-582">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="e8f08-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="e8f08-583">После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="e8f08-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="e8f08-584">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="e8f08-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="e8f08-585">![Выбрав новый веб-сайт](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="e8f08-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="e8f08-586">*Выбрав новый веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="e8f08-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="e8f08-587">![Веб-сайт работал](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "веб-узлом")</span><span class="sxs-lookup"><span data-stu-id="e8f08-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="e8f08-588">*Веб-сайт под управлением*</span><span class="sxs-lookup"><span data-stu-id="e8f08-588">*Web site running*</span></span>
6. <span data-ttu-id="e8f08-589">Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="e8f08-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="e8f08-590">![Открытие страницы управления веб-сайт](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Открытие страницы управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="e8f08-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="e8f08-591">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="e8f08-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="e8f08-592">В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="e8f08-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-593">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайт Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="e8f08-594">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации.</span><span class="sxs-lookup"><span data-stu-id="e8f08-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="e8f08-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений на веб-сайтах Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f08-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="e8f08-596">![Профиль публикации веб-сайт загрузки](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="e8f08-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="e8f08-597">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="e8f08-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="e8f08-598">Скачайте файл профиля публикации в известном месте.</span><span class="sxs-lookup"><span data-stu-id="e8f08-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="e8f08-599">Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения на веб-сайтах, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8f08-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="e8f08-600">![Сохранение файла профиля публикации](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="e8f08-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="e8f08-601">*Сохранение файла профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="e8f08-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="e8f08-602">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="e8f08-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="e8f08-603">Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="e8f08-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="e8f08-604">Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e8f08-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="e8f08-605">Вам потребуется сервер базы данных SQL для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="e8f08-606">Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="e8f08-607">Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="e8f08-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="e8f08-608">Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="e8f08-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="e8f08-609">Не создавайте базы данных, так как он будет создан в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="e8f08-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="e8f08-610">![Панель мониторинга базы данных SQL Server](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="e8f08-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="e8f08-611">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="e8f08-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="e8f08-612">В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="e8f08-613">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля.</span><span class="sxs-lookup"><span data-stu-id="e8f08-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="e8f08-614">Введите имя для правила и нажмите кнопку ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="e8f08-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Добавление IP-адрес клиента](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="e8f08-616">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="e8f08-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="e8f08-617">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="e8f08-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="e8f08-619">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="e8f08-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e8f08-620">Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="e8f08-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="e8f08-621">Вернитесь к решению для ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e8f08-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="e8f08-622">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="e8f08-623">![Публикация приложения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "публикации приложения")</span><span class="sxs-lookup"><span data-stu-id="e8f08-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="e8f08-624">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="e8f08-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="e8f08-625">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="e8f08-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="e8f08-626">![Импорт профиля публикации](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="e8f08-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="e8f08-627">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="e8f08-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="e8f08-628">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-628">Click **Validate Connection**.</span></span> <span data-ttu-id="e8f08-629">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8f08-630">Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.</span><span class="sxs-lookup"><span data-stu-id="e8f08-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="e8f08-631">![Проверка подключения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="e8f08-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="e8f08-632">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="e8f08-632">*Validating connection*</span></span>
4. <span data-ttu-id="e8f08-633">В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="e8f08-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="e8f08-634">![Конфигурация веб-развертывания](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="e8f08-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="e8f08-635">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="e8f08-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="e8f08-636">Настройте подключение к базе данных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e8f08-636">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="e8f08-637">В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="e8f08-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="e8f08-638">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="e8f08-638">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="e8f08-639">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="e8f08-639">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="e8f08-640">Введите новое имя базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="e8f08-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="e8f08-641">![Настройка строки подключения назначения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Настройка строка соединения с назначением")</span><span class="sxs-lookup"><span data-stu-id="e8f08-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="e8f08-642">*Настройка строки подключения назначения*</span><span class="sxs-lookup"><span data-stu-id="e8f08-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="e8f08-643">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-643">Then click **OK**.</span></span> <span data-ttu-id="e8f08-644">При появлении запроса на создание базы данных нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="e8f08-645">![Создание базы данных](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="e8f08-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="e8f08-646">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="e8f08-646">*Creating the database*</span></span>
7. <span data-ttu-id="e8f08-647">В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f08-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="e8f08-648">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-648">Then click **Next**.</span></span>

    <span data-ttu-id="e8f08-649">![Строка подключения, указывающий на базу данных SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "строку подключения, указывающий на базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="e8f08-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="e8f08-650">*Строка подключения, указывающий на базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="e8f08-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="e8f08-651">В **предварительной версии** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e8f08-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="e8f08-652">![Публикация веб-приложения](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="e8f08-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="e8f08-653">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="e8f08-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="e8f08-654">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="e8f08-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="e8f08-655">![Приложение опубликовано в Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e8f08-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="e8f08-656">*Приложение будет публиковаться в Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="e8f08-656">*Application published to Windows Azure*</span></span>
