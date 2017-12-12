---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "С помощью инспектора страниц для Visual Studio 2012 в ASP.NET Web Forms | Документы Microsoft"
author: rick-anderson
description: "Инспектор страниц для Visual Studio 2012 — средство веб-разработки с интегрированным браузером. Выберите любой элемент в встроенном браузере и инспектор страниц..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: a2ac8334e62e6ab7af7042572cfd5950c687001b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="15b4d-104">С помощью инспектора страниц для Visual Studio 2012 в веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15b4d-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="15b4d-105">по Тим Ammann</span><span class="sxs-lookup"><span data-stu-id="15b4d-105">by Tim Ammann</span></span>

> <span data-ttu-id="15b4d-106">Инспектор страниц для Visual Studio 2012 — средство веб-разработки с интегрированным браузером.</span><span class="sxs-lookup"><span data-stu-id="15b4d-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="15b4d-107">Выберите любой элемент во встроенном браузере и инспектор страниц мгновенно выделяет исходного элемента и CSS.</span><span class="sxs-lookup"><span data-stu-id="15b4d-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="15b4d-108">Можно просматривать все страницы в приложении, быстро найти источники для отображения разметки и использовать средства браузера непосредственно в среде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15b4d-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="15b4d-109">Этот учебник shwos как включить режим проверки и быстро найти и отредактировать правил CSS и текста внутри веб-проекта.</span><span class="sxs-lookup"><span data-stu-id="15b4d-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="15b4d-110">В этом учебнике используются проект веб-форм приложения, но можно также использовать инспектор страниц для проектов веб-сайтов и [MVC](https://go.microsoft.com/?linkid=9802002) приложений.</span><span class="sxs-lookup"><span data-stu-id="15b4d-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="15b4d-111">Учебник содержит следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="15b4d-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="15b4d-112">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="15b4d-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="15b4d-113">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="15b4d-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="15b4d-114">Для просмотра приложения с помощью инспектора страниц</span><span class="sxs-lookup"><span data-stu-id="15b4d-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="15b4d-115">Включить режим инспектирования</span><span class="sxs-lookup"><span data-stu-id="15b4d-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="15b4d-116">Для внесения изменений в разметке использовать инспектор страниц</span><span class="sxs-lookup"><span data-stu-id="15b4d-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="15b4d-117">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="15b4d-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="15b4d-118">Просмотр изменений CSS в окне «Стили»</span><span class="sxs-lookup"><span data-stu-id="15b4d-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="15b4d-119">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="15b4d-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="15b4d-120">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="15b4d-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="15b4d-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="15b4d-121">Prerequisites</span></span>

- <span data-ttu-id="15b4d-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) или [Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="15b4d-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="15b4d-123">Чтобы получить последнюю версию инспектор страниц, используйте [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) для установки пакета Azure SDK для .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="15b4d-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="15b4d-124">Инспектор страниц входит в состав средств веб-разработки Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="15b4d-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="15b4d-125">Последняя версия — 1.3.</span><span class="sxs-lookup"><span data-stu-id="15b4d-125">The latest version is 1.3.</span></span> <span data-ttu-id="15b4d-126">Проверка версии имеют, запустите Visual Studio и выберите **о Microsoft Visual Studio** из **справки** меню.</span><span class="sxs-lookup"><span data-stu-id="15b4d-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="15b4d-127">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="15b4d-127">Create a Web Application</span></span>

<span data-ttu-id="15b4d-128">Во-первых вы создадите веб-приложения, который будет использоваться инспектор страниц с.</span><span class="sxs-lookup"><span data-stu-id="15b4d-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="15b4d-129">В Visual Studio выберите **файл** &gt; **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="15b4d-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="15b4d-130">Слева разверните узел **Visual C#**выберите **веб**, а затем выберите **приложениях ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="15b4d-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Новое приложение Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="15b4d-132">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="15b4d-132">Click **OK**.</span></span>

<span data-ttu-id="15b4d-133">Приложение откроется в **источника** представления.</span><span class="sxs-lookup"><span data-stu-id="15b4d-133">The application opens in **Source** view.</span></span>

![Новое веб-приложение форм в режиме исходного кода](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="15b4d-135">Теперь, когда приложение для работы с инспектор страниц можно использовать для проверки и изменения его.</span><span class="sxs-lookup"><span data-stu-id="15b4d-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="15b4d-136">Для просмотра приложения с помощью инспектора страниц</span><span class="sxs-lookup"><span data-stu-id="15b4d-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="15b4d-137">Далее будет просмотреть приложение в инспекторе страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="15b4d-138">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **просмотреть в инспекторе страниц**.</span><span class="sxs-lookup"><span data-stu-id="15b4d-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Просмотреть в инспекторе страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="15b4d-140">По умолчанию при запуске инспектор страниц в первый раз его закреплены как узких окно слева от среды Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15b4d-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="15b4d-141">Оставьте его закреплены в левой части и присвойте ему значение ширины, которая удобства или закрепить его в одной из областей средство верхней, нижней или справа:</span><span class="sxs-lookup"><span data-stu-id="15b4d-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Инспектор страниц закрепления позиций](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="15b4d-143">Если вы открепить окно инспектора страниц, его можно поместить вне Visual Studio, или даже на втором мониторе если таковая имеется.</span><span class="sxs-lookup"><span data-stu-id="15b4d-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="15b4d-144">Тем не менее, чтобы сочетание клавиш ALT + TAB между инспектор страниц и Visual Studio при окна инспектора страниц не закреплена, перейдите к **средства** &gt; **параметры** &gt;  **Среда** &gt; **вкладки и окна**и в разделе **вкладка также**, снимите флажок называется **плавающие окна инструментов всегда остаются на основе Главное окно**:</span><span class="sxs-lookup"><span data-stu-id="15b4d-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Снимите флажок с плавающей запятой средство windows ALT + TAB между Visual Studio и Незакрепленное окно инспектора страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="15b4d-146">В верхней области окна инспектора страниц показываются текущей страницы в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="15b4d-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="15b4d-147">На нижней панели отображаются страницы в разметке HTML в левой части экрана, а некоторые вкладки справа, где можно просмотреть различные аспекты страницы.</span><span class="sxs-lookup"><span data-stu-id="15b4d-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="15b4d-148">На нижней панели аналогичен [средств разработчика F12](https://msdn.microsoft.com/en-us/ie/aa740478) в Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="15b4d-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/en-us/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="15b4d-149">(Тем не менее, в отличие от средства разработчика можно использовать инспектор страниц непосредственно в Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="15b4d-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Инспектор страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="15b4d-151">В этом учебнике будет использоваться панели браузера инспектора страниц и **HTML** и **стили** вкладок, чтобы помочь быстро перемещаться и внести изменения в приложение.</span><span class="sxs-lookup"><span data-stu-id="15b4d-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="15b4d-152">Включить режим инспектирования</span><span class="sxs-lookup"><span data-stu-id="15b4d-152">Enable Inspection Mode</span></span>

<span data-ttu-id="15b4d-153">Далее вы увидите, как работает режим инспектирования инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="15b4d-154">В окне инспектора страниц щелкните **инспектировать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="15b4d-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Проверки элемента](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="15b4d-156">Чтобы просмотреть режим инспектирования в действии, наведите курсор на различные части страницы в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="15b4d-157">После этого указатель мыши изменяется знака плюс и будет выделен под элементом:</span><span class="sxs-lookup"><span data-stu-id="15b4d-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Наведении указателя мыши на div.content оболочки](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="15b4d-159">При перемещении указателя мыши, обратите внимание, что</span><span class="sxs-lookup"><span data-stu-id="15b4d-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="15b4d-160">Содержимое в **источника** просмотра изменяется для отображения разметки, соответствующие выбранному элементу страницы.</span><span class="sxs-lookup"><span data-stu-id="15b4d-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="15b4d-161">Соответствующие разметка выделяется цветом.</span><span class="sxs-lookup"><span data-stu-id="15b4d-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="15b4d-162">Если источником является в другом файле, этот файл открывается в режиме исходного кода с выделяются соответствующую разметку.</span><span class="sxs-lookup"><span data-stu-id="15b4d-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="15b4d-163">Разметка, отображаемая в **HTML** вкладку в инспекторе страниц также изменяется в соответствии с выбранного элемента на странице.</span><span class="sxs-lookup"><span data-stu-id="15b4d-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="15b4d-164">В **HTML** вкладки, описанные соответствующую разметку.</span><span class="sxs-lookup"><span data-stu-id="15b4d-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="15b4d-165">**Стили** вкладке отображаются правила CSS, относящиеся к текущему выбранному элементу.</span><span class="sxs-lookup"><span data-stu-id="15b4d-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="15b4d-166">Для внесения изменений в разметке использовать инспектор страниц</span><span class="sxs-lookup"><span data-stu-id="15b4d-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="15b4d-167">Теперь вы увидите, как можно использовать для поиска и внести изменения в разметке или текст, расположение которого может оказаться неочевидным инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="15b4d-168">Перевести инспектор страниц в режиме инспектирования, а затем прокрутите до нижней части домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="15b4d-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="15b4d-169">Как только вводится области нижнего колонтитула, инспектор страниц будет открывать *Site.Master* файл макета в **источника** представление на временной вкладке справа от другой вкладки и выделяет раздел главного страницы, которые выбраны.</span><span class="sxs-lookup"><span data-stu-id="15b4d-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="15b4d-170">Это показывает, как инспектор страниц можно найти и просмотреть содержимое на странице, которая фактически могут поступать из другого файла, чем тот, который был открыт изначально.</span><span class="sxs-lookup"><span data-stu-id="15b4d-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Выделяет нижнего колонтитула в режиме инспектирования](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="15b4d-172">В окне браузера инспектора страниц наведите указатель мыши на строку с авторских прав <a id="a"> </a>Обратите внимание.</span><span class="sxs-lookup"><span data-stu-id="15b4d-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="15b4d-173">В *Site.Master* страницы, соответствующая строка будет выделена.</span><span class="sxs-lookup"><span data-stu-id="15b4d-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Выделенной об авторских правах строке нижнего колонтитула](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="15b4d-175">Добавьте в конец строки в текст *Site.Master* файла.</span><span class="sxs-lookup"><span data-stu-id="15b4d-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="15b4d-176">&lt;p&gt;&amp;копирование; &lt;%: DateTime.Now.Year %&gt; -Rocks приложение My ASP.NET!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="15b4d-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="15b4d-177">Сейчас нажмите клавиши Ctrl + Alt + Ввод или щелкните панель обновления, чтобы просмотреть результаты в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Мои Rocks приложения ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="15b4d-179">Вы могли подумать, нижний колонтитул был на *Default.aspx* страницы, но принявшее значение master макет страницы и инспектор страниц найти его автоматически.</span><span class="sxs-lookup"><span data-stu-id="15b4d-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="15b4d-180">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="15b4d-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="15b4d-181">Затем необходимо будет кратко рассмотрим окна HTML и как они соответствуют элементы для вас.</span><span class="sxs-lookup"><span data-stu-id="15b4d-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="15b4d-182">Перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="15b4d-182">Put Page Inspector in Inspection Mode.</span></span>

![Проверки элемента](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="15b4d-184">Нажмите кнопку в верхней части страницы с надписью «логотип вашей организации».</span><span class="sxs-lookup"><span data-stu-id="15b4d-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="15b4d-185">Просматриваемую конкретный элемент более подробно, поэтому отображения в окне браузера больше не изменяется при перемещении указателя мыши.</span><span class="sxs-lookup"><span data-stu-id="15b4d-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="15b4d-186">Теперь наведите указатель мыши на **HTML** окна.</span><span class="sxs-lookup"><span data-stu-id="15b4d-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="15b4d-187">При перемещении указателя мыши инспектор страниц описаны элемент внутри **HTML** окна и выделяет соответствующий элемент в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="15b4d-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Окно HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="15b4d-189">Как и прежде, инспектор страниц будет открывать *Site.Master* файл на временной вкладке. Перейдите на вкладку Site.Master, и соответствующая разметка выделяется в &lt;заголовок&gt; раздела:</span><span class="sxs-lookup"><span data-stu-id="15b4d-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Выделенная разметки](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="15b4d-191">Просмотр изменений CSS в окне «Стили»</span><span class="sxs-lookup"><span data-stu-id="15b4d-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="15b4d-192">Далее вы увидите, как можно использовать инспектор страниц **стили** окно для предварительного просмотра изменения CSS.</span><span class="sxs-lookup"><span data-stu-id="15b4d-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="15b4d-193">Нажмите кнопку **инспектировать** кнопку, чтобы перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="15b4d-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="15b4d-194">В окне браузера инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** метка отображается.</span><span class="sxs-lookup"><span data-stu-id="15b4d-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Наведение на элементы](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="15b4d-196">Щелкните в разделе div.content оболочку, а затем наведите указатель мыши на **стили** окна.</span><span class="sxs-lookup"><span data-stu-id="15b4d-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="15b4d-197">В группе селектора классов оболочки .content .featured снимите и установите флажок для свойства цвет фона.</span><span class="sxs-lookup"><span data-stu-id="15b4d-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Цвет фона, снимите флажок](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="15b4d-199">Обратите внимание на то, как изменение Предварительный просмотр мгновенно в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="15b4d-200">Снова установите флажок, а затем дважды щелкните значение свойства и измените его на `red`.</span><span class="sxs-lookup"><span data-stu-id="15b4d-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="15b4d-201">Изменения немедленно показано:</span><span class="sxs-lookup"><span data-stu-id="15b4d-201">The change shows immediately:</span></span>

![Красного цвета фона](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="15b4d-203">**Стили** облегчает тестирование и предварительного просмотра CSS изменяется до фиксации изменений стиль делает окно листа сам.</span><span class="sxs-lookup"><span data-stu-id="15b4d-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="15b4d-204">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="15b4d-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="15b4d-205">Этот компонент требуется версия 1.3 инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="15b4d-206">Функция автоматической синхронизации CSS позволяет напрямую изменить CSS-файл и увидеть изменения непосредственно в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="15b4d-207">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="15b4d-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="15b4d-208">В браузере инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** метка отображается.</span><span class="sxs-lookup"><span data-stu-id="15b4d-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="15b4d-209">Щелкните один раз, чтобы выбрать этот элемент.</span><span class="sxs-lookup"><span data-stu-id="15b4d-209">Click once to select this element.</span></span>

<span data-ttu-id="15b4d-210">**Syles** окне показаны все правила CSS для этого элемента.</span><span class="sxs-lookup"><span data-stu-id="15b4d-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="15b4d-211">Прокрутите вниз до селектор класса поиска .featured .content оболочки.</span><span class="sxs-lookup"><span data-stu-id="15b4d-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="15b4d-212">Щелкните «.featured .content-программу-оболочку».</span><span class="sxs-lookup"><span data-stu-id="15b4d-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="15b4d-213">Инспектор страниц открывает CSS-файл, определяющий этот стиль (Site.css) и выделяет соответствующего стиля CSS.</span><span class="sxs-lookup"><span data-stu-id="15b4d-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS-файл](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="15b4d-215">Теперь измените значение `background-color` для «red».</span><span class="sxs-lookup"><span data-stu-id="15b4d-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="15b4d-216">Изменение немедленно отображается в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="15b4d-216">The change appears immediately in the Page Inspector browser.</span></span>

![Браузера инспектора страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="15b4d-218">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="15b4d-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="15b4d-219">Далее вы узнаете, как использовать инспектор страниц для быстрого поиска и изменить следующий код CSS для выделенного текста в приложении по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="15b4d-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="15b4d-220">В этом примере вы решили, что не нравится синим цветом и изменить его на другой цвет.</span><span class="sxs-lookup"><span data-stu-id="15b4d-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="15b4d-221">Нажмите кнопку **инспектировать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="15b4d-221">Click the **Inspect** button.</span></span>

![Проверки элемента](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="15b4d-223">В окне браузера инспектора страниц наведите указатель мыши на выделенный «видео, учебники и образцы», чтобы CSS «пометьте тип» метка будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="15b4d-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Если навести курсор мыши над элементом метки](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="15b4d-225">Щелкните текст, чтобы выбрать его.</span><span class="sxs-lookup"><span data-stu-id="15b4d-225">Click the text to select it.</span></span> <span data-ttu-id="15b4d-226">Соответствующий знак селектора CSS появляется в нижней части **стили** окна.</span><span class="sxs-lookup"><span data-stu-id="15b4d-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Пометить область выделения в окне «Стили»](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="15b4d-228">Щелкните знак выделения.</span><span class="sxs-lookup"><span data-stu-id="15b4d-228">Click the mark selector.</span></span> <span data-ttu-id="15b4d-229">При этом откроется *Site.css* файл веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="15b4d-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="15b4d-230">Перейдите на вкладку Site.css и выделяется соответствующий CSS для селектора.</span><span class="sxs-lookup"><span data-stu-id="15b4d-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Пометить область выделения в таблице стилей](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="15b4d-232">Выберите и удалите строку со свойством цвет фона.</span><span class="sxs-lookup"><span data-stu-id="15b4d-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="15b4d-233">Вы сможете использовать новый палитра цветов Visual Studio 2012 CSS для выберите новый цвет **пометить** свойство цвет фона.</span><span class="sxs-lookup"><span data-stu-id="15b4d-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="15b4d-234">С помощью Visual Studio 2012 палитра CSS</span><span class="sxs-lookup"><span data-stu-id="15b4d-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="15b4d-235">Редактор CSS в Visual Studio 2012 имеет палитру цветов, позволяющие легко выбрать и вставить цвета.</span><span class="sxs-lookup"><span data-stu-id="15b4d-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="15b4d-236">Она имеет простой цветовую шкалу и выбора «pop вниз», которое обеспечивает более точное управление.</span><span class="sxs-lookup"><span data-stu-id="15b4d-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="15b4d-237">Палитра содержит стандартную палитру цветов и поддерживает стандартные имена цветов, хэш-кодов, цветов RGB, RGBA, HSL и HSLA ведет список цветов, которые вы недавно использовали в документе.</span><span class="sxs-lookup"><span data-stu-id="15b4d-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="15b4d-238">В строке, где свойство цвет фона было введите «bc» и нажмите клавишу со стрелкой вниз один раз.</span><span class="sxs-lookup"><span data-stu-id="15b4d-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="15b4d-239">При вводе первый символ каждого слова в свойства, разделенных дефисом, такого как «цвет фона» IntelliSense список будет отфильтрован и отображать только свойства, которые соответствуют:</span><span class="sxs-lookup"><span data-stu-id="15b4d-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense отфильтрованные значения](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="15b4d-241">Теперь введите двоеточие.</span><span class="sxs-lookup"><span data-stu-id="15b4d-241">Now type a colon.</span></span> <span data-ttu-id="15b4d-242">При этом вставляется имя свойства полный цвет фона.</span><span class="sxs-lookup"><span data-stu-id="15b4d-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="15b4d-243">Тип  **#**  или **rgb (**, и отображается палитра цветов:</span><span class="sxs-lookup"><span data-stu-id="15b4d-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Палитра цветов CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="15b4d-245">Чтобы увидеть, как работает средство выбора цветов, щелкните его цвета с указателем, или нажмите клавишу Стрелка вниз и затем с помощью клавиш со стрелками влево и вправо для обхода цвета.</span><span class="sxs-lookup"><span data-stu-id="15b4d-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="15b4d-246">При посещении цвет, соответствующее значение для свойства цвет фона предварительного просмотра:</span><span class="sxs-lookup"><span data-stu-id="15b4d-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![значение свойства цвет фона предварительного просмотра](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="15b4d-248">На этом этапе можно нажать ВВОД, чтобы выбрать значения, а затем точку с запятой (;) для завершения операции CSS.</span><span class="sxs-lookup"><span data-stu-id="15b4d-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="15b4d-249">Теперь перейдите к разделу, чтобы вы увидите, как работает средство выбора цвета pop раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="15b4d-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="15b4d-250">С помощью средства выбора цвета Pop раскрывающегося списка</span><span class="sxs-lookup"><span data-stu-id="15b4d-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="15b4d-251">При цветовой шкалы не имеет точного цвет, который вы ищете, можно использовать палитру цветов pop раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="15b4d-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="15b4d-252">Чтобы открыть его, щелкните значок шеврона в правом конце цветовой шкалы или один или два раза нажмите клавишу Стрелка вниз на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="15b4d-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Палитра цветов CSS Pop раскрывающегося списка](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="15b4d-254">Выберите цвет из вертикальная черта справа.</span><span class="sxs-lookup"><span data-stu-id="15b4d-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="15b4d-255">Здесь показан градиент цвета в главном окне.</span><span class="sxs-lookup"><span data-stu-id="15b4d-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="15b4d-256">Выберите цвет непосредственно на вертикальную панель, нажав клавишу ВВОД или щелкнув любую из точек в главном окне выберите с большей точностью.</span><span class="sxs-lookup"><span data-stu-id="15b4d-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="15b4d-257">Существует ли цвет на экране компьютера, который вы хотите использовать (это не обязательно должно находиться в пользовательском интерфейсе Visual Studio), его значение можно записать с помощью пипетки в правом нижнем углу.</span><span class="sxs-lookup"><span data-stu-id="15b4d-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="15b4d-258">Также можно изменить степени непрозрачности цвета, переместив ползунок в нижней части палитру цветов.</span><span class="sxs-lookup"><span data-stu-id="15b4d-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="15b4d-259">Таким образом, изменения значения RGBA значения цветов, так как формат RGBA может представлять непрозрачности.</span><span class="sxs-lookup"><span data-stu-id="15b4d-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="15b4d-260">После выбора цвета, нажмите клавишу ВВОД, а затем введите точку с запятой для завершения операции цвет фона в *Site.css* файла.</span><span class="sxs-lookup"><span data-stu-id="15b4d-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="15b4d-261">Панель обновления инспектора страницы</span><span class="sxs-lookup"><span data-stu-id="15b4d-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="15b4d-262">Инспектор страниц немедленно обнаруживает изменение *Site.css* файла (или любой другой файл в приложении) и отображает предупреждение на панели обновления.</span><span class="sxs-lookup"><span data-stu-id="15b4d-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Панель обновления](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="15b4d-264">Чтобы сохранить все файлы и обновить страницу в браузере инспектора страниц, нажмите сочетание клавиш Ctrl + Alt + Ввод или щелкните панель обновления.</span><span class="sxs-lookup"><span data-stu-id="15b4d-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="15b4d-265">Изменение цвета выделения откроется в браузере:</span><span class="sxs-lookup"><span data-stu-id="15b4d-265">The change in the highlight color appears in the browser:</span></span>

![Изменить цвет выделения](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="15b4d-267">Обратите внимание, удобным образом обновить браузер инспектора страниц прямо из среды Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15b4d-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="15b4d-268">С помощью инспектора страниц вместо внешнем браузере позволяет оставаться в редакторе, при разработке веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="15b4d-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="15b4d-269">См. также</span><span class="sxs-lookup"><span data-stu-id="15b4d-269">See Also</span></span>

<span data-ttu-id="15b4d-270">[Введение в инспектор страниц](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (видео на канале 9)</span><span class="sxs-lookup"><span data-stu-id="15b4d-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="15b4d-271">[Сообщения об ошибках инспектора страниц](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="15b4d-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
