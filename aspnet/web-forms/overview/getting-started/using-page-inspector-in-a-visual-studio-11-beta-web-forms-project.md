---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Использование инспектора страниц для Visual Studio 2012 в ASP.NET Web Forms | Документация Майкрософт
author: rick-anderson
description: Инспектор страниц для Visual Studio 2012 — это средство веб-разработки с интегрированным браузером. Выберите любой элемент в интегрированной браузером и инспектор страниц...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1f1ac1072d33c85ed3e64c493b9cf7970695cea6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384479"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="19fa1-104">Использование инспектора страниц для Visual Studio 2012 в веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="19fa1-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="19fa1-105">Тим Ammann</span><span class="sxs-lookup"><span data-stu-id="19fa1-105">by Tim Ammann</span></span>

> <span data-ttu-id="19fa1-106">Инспектор страниц для Visual Studio 2012 — это средство веб-разработки с интегрированным браузером.</span><span class="sxs-lookup"><span data-stu-id="19fa1-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="19fa1-107">Выберите любой элемент в встроенного браузера и инспектор страниц мгновенно выделения источника этого элемента и CSS.</span><span class="sxs-lookup"><span data-stu-id="19fa1-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="19fa1-108">Можно просмотреть любой страницы в приложении, быстро находить источники визуализированной разметке и использовать средства браузера непосредственно в среде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fa1-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="19fa1-109">Этот учебник shwos как включить режим инспектирования и быстро найти и отредактировать правила CSS и текста в веб-проект.</span><span class="sxs-lookup"><span data-stu-id="19fa1-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="19fa1-110">В этом руководстве используется проект веб-форм приложения, но можно также использовать инспектор страниц для проектов веб-сайтов и [MVC](https://go.microsoft.com/?linkid=9802002) приложений.</span><span class="sxs-lookup"><span data-stu-id="19fa1-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="19fa1-111">Учебник содержит следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="19fa1-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="19fa1-112">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="19fa1-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="19fa1-113">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="19fa1-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="19fa1-114">Использование инспектора страниц для просмотра приложения</span><span class="sxs-lookup"><span data-stu-id="19fa1-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="19fa1-115">Включить режим проверки</span><span class="sxs-lookup"><span data-stu-id="19fa1-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="19fa1-116">Использование инспектора страниц вносить изменения в разметку</span><span class="sxs-lookup"><span data-stu-id="19fa1-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="19fa1-117">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="19fa1-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="19fa1-118">Просмотр изменений CSS в окне "Стили"</span><span class="sxs-lookup"><span data-stu-id="19fa1-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="19fa1-119">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="19fa1-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="19fa1-120">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="19fa1-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="19fa1-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="19fa1-121">Prerequisites</span></span>

- <span data-ttu-id="19fa1-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) или [Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="19fa1-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="19fa1-123">Чтобы получить последнюю версию инспектор страниц, используйте [установщика веб-платформы](https://go.microsoft.com/fwlink/?LinkId=255386) для установки пакета Azure SDK для .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="19fa1-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="19fa1-124">Инспектор страниц входит в состав инструменты разработчика Microsoft Web.</span><span class="sxs-lookup"><span data-stu-id="19fa1-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="19fa1-125">Последняя версия — 1.3.</span><span class="sxs-lookup"><span data-stu-id="19fa1-125">The latest version is 1.3.</span></span> <span data-ttu-id="19fa1-126">Чтобы проверить, какая версия у, запустите Visual Studio и выберите **о Microsoft Visual Studio** из **помочь** меню.</span><span class="sxs-lookup"><span data-stu-id="19fa1-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="19fa1-127">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="19fa1-127">Create a Web Application</span></span>

<span data-ttu-id="19fa1-128">Во-первых вы создадите веб-приложения, который будет использоваться инспектор страниц с.</span><span class="sxs-lookup"><span data-stu-id="19fa1-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="19fa1-129">В Visual Studio, выберите **файл** &gt; **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="19fa1-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="19fa1-130">Слева разверните **Visual C#** выберите **Web**, а затем выберите **приложения веб-форм ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="19fa1-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Новое приложение Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="19fa1-132">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="19fa1-132">Click **OK**.</span></span>

<span data-ttu-id="19fa1-133">Приложение откроется в **источника** представления.</span><span class="sxs-lookup"><span data-stu-id="19fa1-133">The application opens in **Source** view.</span></span>

![Новые приложения веб-форм в представлении источника](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="19fa1-135">Теперь, когда у вас есть приложение для работы с, инспектор страниц можно использовать для проверки и изменения его.</span><span class="sxs-lookup"><span data-stu-id="19fa1-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="19fa1-136">Использование инспектора страниц для просмотра приложения</span><span class="sxs-lookup"><span data-stu-id="19fa1-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="19fa1-137">Далее вы просмотрите приложение с помощью инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="19fa1-138">В **обозревателе решений**, правой кнопкой мыши проект и выберите **просмотреть в инспекторе страниц**.</span><span class="sxs-lookup"><span data-stu-id="19fa1-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Просмотреть в инспекторе страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="19fa1-140">По умолчанию при запуске инспектора страниц в первый раз закрепляется как узких окна на левой стороне среды Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fa1-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="19fa1-141">Оставьте закрепленной в левой части и присвойте ему значение, ширина которого удобное для вас или закрепить его в одной из областей средство верхней, нижней или справа:</span><span class="sxs-lookup"><span data-stu-id="19fa1-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Положения закрепления инспектор страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="19fa1-143">Если вы открепить окно инспектора страниц, его можно поместить за пределами Visual Studio, или даже на втором мониторе если таковой имеется.</span><span class="sxs-lookup"><span data-stu-id="19fa1-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="19fa1-144">Тем не менее, чтобы ALT + TAB между инспектор страниц и Visual Studio при открепленного окна инспектора страниц, перейдите к **средства** &gt; **параметры** &gt;  **Среда** &gt; **вкладки и Windows**и в разделе **вкладке также**, снимите флажок называется **окна инструментов с плавающей запятой всегда остаются в верхней части Главное окно**:</span><span class="sxs-lookup"><span data-stu-id="19fa1-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Снимите флажок с плавающей запятой windows средство ALT + TAB, между Visual Studio и открепленного окна инспектора страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="19fa1-146">В верхней области окна инспектора страниц показываются текущей страницы в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="19fa1-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="19fa1-147">На нижней панели отображаются страницы в разметке HTML в левой части, а некоторые вкладки в правой части, которые позволяют проверять различные аспекты страницы.</span><span class="sxs-lookup"><span data-stu-id="19fa1-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="19fa1-148">На нижней панели аналогичен [средства разработчика F12](https://msdn.microsoft.com/ie/aa740478) в Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="19fa1-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="19fa1-149">(Тем не менее, в отличие от средств разработчика, можно использовать инспектор страниц непосредственно в Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="19fa1-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Инспектор страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="19fa1-151">В этом руководстве используется на панели браузера инспектора страниц и **HTML** и **стили** вкладок, чтобы помочь быстро перейти и внести изменения в приложение.</span><span class="sxs-lookup"><span data-stu-id="19fa1-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="19fa1-152">Включить режим проверки</span><span class="sxs-lookup"><span data-stu-id="19fa1-152">Enable Inspection Mode</span></span>

<span data-ttu-id="19fa1-153">Далее вы увидите, как работает режим инспектирования инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="19fa1-154">В окне инспектора страниц щелкните **инспектировать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="19fa1-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Проверить элемент](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="19fa1-156">Чтобы просмотреть режим инспектирования в действии, в различные части страницы в окне браузера инспектора страниц наведите указатель мыши.</span><span class="sxs-lookup"><span data-stu-id="19fa1-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="19fa1-157">После этого указатель мыши примет знака плюс, и выделяется под элементом:</span><span class="sxs-lookup"><span data-stu-id="19fa1-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![При наведении div.content оболочки](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="19fa1-159">При перемещении указателя мыши, обратите внимание, что</span><span class="sxs-lookup"><span data-stu-id="19fa1-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="19fa1-160">Содержимое в **источника** просмотра изменяется для отображения разметки, соответствующий выбранному элементу на странице.</span><span class="sxs-lookup"><span data-stu-id="19fa1-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="19fa1-161">Соответствующая разметка выделяется.</span><span class="sxs-lookup"><span data-stu-id="19fa1-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="19fa1-162">Если источник данных находится в другой файл, этот файл открывается в представлении источника соответствующие выделенным разметкой.</span><span class="sxs-lookup"><span data-stu-id="19fa1-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="19fa1-163">Разметка, отображаемая в **HTML** вкладку в инспекторе страниц также изменяется в соответствии с выбранного элемента на странице.</span><span class="sxs-lookup"><span data-stu-id="19fa1-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="19fa1-164">В **HTML** вкладке описана соответствующая разметка.</span><span class="sxs-lookup"><span data-stu-id="19fa1-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="19fa1-165">**Стили** вкладке отображаются правила CSS, соответствующие к текущему выделению.</span><span class="sxs-lookup"><span data-stu-id="19fa1-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="19fa1-166">Использование инспектора страниц вносить изменения в разметку</span><span class="sxs-lookup"><span data-stu-id="19fa1-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="19fa1-167">Теперь вы увидите, как инспектор страниц можно использовать для поиска и внести изменения в разметку или текст, расположение которой может не всегда самоочевиден.</span><span class="sxs-lookup"><span data-stu-id="19fa1-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="19fa1-168">Перевести инспектор страниц в режиме инспектирования, а затем прокрутите до нижней части домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="19fa1-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="19fa1-169">Как только вы введете области нижнего колонтитула, инспектор страниц открывает *Site.Master* файл макета в **источника** представления в временную вкладку справа от другой вкладки и выделяет часть главной странице, выбраны.</span><span class="sxs-lookup"><span data-stu-id="19fa1-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="19fa1-170">Это показано, как инспектор страниц можно найти и отображения содержимого на страницу, которая может действительно получены от той, которую была открыта другой файл.</span><span class="sxs-lookup"><span data-stu-id="19fa1-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Основные особенности нижний колонтитул в режиме инспектирования](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="19fa1-172">В окне браузера инспектора страниц наведите указатель мыши на строку с авторских прав <a id="a"> </a>Обратите внимание, что.</span><span class="sxs-lookup"><span data-stu-id="19fa1-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="19fa1-173">В *Site.Master* страницы, соответствующая строка будет выделена.</span><span class="sxs-lookup"><span data-stu-id="19fa1-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Строка нижнего колонтитула об авторских правах, выделяемая подсветкой](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="19fa1-175">Добавьте какой-нибудь текст в конец строки в *Site.Master* файл.</span><span class="sxs-lookup"><span data-stu-id="19fa1-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="19fa1-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; — My ASP.NET Application Rocks!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="19fa1-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="19fa1-177">Теперь нажмите клавиши Ctrl + Alt + Ввод или щелкните панель обновления, чтобы просмотреть результаты в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="19fa1-179">Вы наверняка считали, что нижний колонтитул был на *Default.aspx* страницы, но оно оказалось в страница с эталонной разметкой, и инспектор страниц нашли для вас.</span><span class="sxs-lookup"><span data-stu-id="19fa1-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="19fa1-180">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="19fa1-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="19fa1-181">Затем вы получите краткий обзор HTML-окну, и способом сопоставления элементов для вас.</span><span class="sxs-lookup"><span data-stu-id="19fa1-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="19fa1-182">Перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="19fa1-182">Put Page Inspector in Inspection Mode.</span></span>

![Проверить элемент](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="19fa1-184">Щелкните в верхней части страницы «ваша эмблема здесь».</span><span class="sxs-lookup"><span data-stu-id="19fa1-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="19fa1-185">Изучении конкретный элемент более подробно, поэтому отображение в окне браузера больше не изменяется при перемещении указателя мыши.</span><span class="sxs-lookup"><span data-stu-id="19fa1-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="19fa1-186">Теперь наведите указатель мыши на **HTML** окна.</span><span class="sxs-lookup"><span data-stu-id="19fa1-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="19fa1-187">При перемещении указателя мыши, инспектор страниц описаны сервисном **HTML** окне и выделяет соответствующий элемент в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="19fa1-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Окно HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="19fa1-189">Как и раньше, инспектор страниц открывает *Site.Master* файл для вас в временную вкладку. Перейдите на вкладку Site.Master, и соответствующая разметка выделяется в &lt;заголовок&gt; раздел:</span><span class="sxs-lookup"><span data-stu-id="19fa1-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Выделенная разметка](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="19fa1-191">Просмотр изменений CSS в окне "Стили"</span><span class="sxs-lookup"><span data-stu-id="19fa1-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="19fa1-192">Далее, вы увидите, как можно использовать инспектор страниц **стили** окно, чтобы просмотреть изменения в CSS.</span><span class="sxs-lookup"><span data-stu-id="19fa1-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="19fa1-193">Нажмите кнопку **инспектировать** кнопку, чтобы перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="19fa1-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="19fa1-194">В окне браузера инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** появится метка.</span><span class="sxs-lookup"><span data-stu-id="19fa1-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Наведение на элементы](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="19fa1-196">Щелкните в разделе div.content оболочку, а затем наведите указатель мыши на **стили** окна.</span><span class="sxs-lookup"><span data-stu-id="19fa1-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="19fa1-197">В группе селектора класса .featured .content оболочки снимите и установите флажок для свойства цвет фона.</span><span class="sxs-lookup"><span data-stu-id="19fa1-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Цвет фона очистить](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="19fa1-199">Обратите внимание на то, как изменение выполняет предварительный просмотр сразу же в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="19fa1-200">Снова установите флажок, а затем дважды щелкните значение свойства и измените его на `red`.</span><span class="sxs-lookup"><span data-stu-id="19fa1-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="19fa1-201">Изменение немедленно показано:</span><span class="sxs-lookup"><span data-stu-id="19fa1-201">The change shows immediately:</span></span>

![Красного цвета фона](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="19fa1-203">**Стили** облегчает тестирование и предварительный просмотр CSS изменения, прежде чем зафиксировать изменения стиля делает окно листа сам.</span><span class="sxs-lookup"><span data-stu-id="19fa1-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="19fa1-204">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="19fa1-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="19fa1-205">Эта функция требует версии 1.3 инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="19fa1-206">Автосинхронизация CSS позволяет напрямую изменить CSS-файл и увидеть изменения прямо в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="19fa1-207">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="19fa1-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="19fa1-208">В браузере инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** появится метка.</span><span class="sxs-lookup"><span data-stu-id="19fa1-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="19fa1-209">Щелкните один раз, чтобы выбрать этот элемент.</span><span class="sxs-lookup"><span data-stu-id="19fa1-209">Click once to select this element.</span></span>

<span data-ttu-id="19fa1-210">**Стили** окне показаны все правила CSS для данного элемента.</span><span class="sxs-lookup"><span data-stu-id="19fa1-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="19fa1-211">Прокрутите вниз до селектор класса поиска .featured .content оболочки.</span><span class="sxs-lookup"><span data-stu-id="19fa1-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="19fa1-212">Щелкните «.featured .content оболочки».</span><span class="sxs-lookup"><span data-stu-id="19fa1-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="19fa1-213">Инспектор страниц открывает CSS-файл, который определяет этот стиль (Site.css) и выделяет соответствующий стиль CSS.</span><span class="sxs-lookup"><span data-stu-id="19fa1-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS-файл](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="19fa1-215">Теперь измените значение `background-color` «красный».</span><span class="sxs-lookup"><span data-stu-id="19fa1-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="19fa1-216">Изменение немедленно отображается в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="19fa1-216">The change appears immediately in the Page Inspector browser.</span></span>

![Браузере инспектора страниц](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="19fa1-218">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="19fa1-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="19fa1-219">Далее вы узнаете, как использовать инспектор страниц поможет быстро найти и изменить код CSS для выделенного текста в приложение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="19fa1-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="19fa1-220">В этом примере вы решили, что вы не такие как выделенной синим цветом области и хотите изменить его на другой цвет.</span><span class="sxs-lookup"><span data-stu-id="19fa1-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="19fa1-221">Нажмите кнопку **инспектировать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="19fa1-221">Click the **Inspect** button.</span></span>

![Проверить элемент](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="19fa1-223">В окне браузера инспектора страниц наведите указатель мыши на выделенный «видео, учебники и образцы», чтобы CSS «пометить» метка будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="19fa1-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Элемент метки при наведении](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="19fa1-225">Щелкните текст, чтобы выбрать его.</span><span class="sxs-lookup"><span data-stu-id="19fa1-225">Click the text to select it.</span></span> <span data-ttu-id="19fa1-226">Соответствующий знак селектора CSS появляется в нижней части **стили** окна.</span><span class="sxs-lookup"><span data-stu-id="19fa1-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Пометить область выделения в окне "Стили"](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="19fa1-228">Щелкните селектор Марк.</span><span class="sxs-lookup"><span data-stu-id="19fa1-228">Click the mark selector.</span></span> <span data-ttu-id="19fa1-229">Откроется *Site.css* файл веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="19fa1-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="19fa1-230">Перейдите на вкладку Site.css, и выделяется соответствующий CSS для селектора:</span><span class="sxs-lookup"><span data-stu-id="19fa1-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Пометить селектора в таблице стилей](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="19fa1-232">Выберите и удалите строку в свойстве цвета фона.</span><span class="sxs-lookup"><span data-stu-id="19fa1-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="19fa1-233">Вы сможете использовать новое средство выбора цвета Visual Studio 2012 CSS для выбора нового цвета для **пометить** свойство цвет фона.</span><span class="sxs-lookup"><span data-stu-id="19fa1-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="19fa1-234">С помощью Visual Studio 2012 палитра цветов CSS</span><span class="sxs-lookup"><span data-stu-id="19fa1-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="19fa1-235">Редактор CSS в Visual Studio 2012 содержит палитру цветов, позволяет легко выбрать и вставить цвета.</span><span class="sxs-lookup"><span data-stu-id="19fa1-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="19fa1-236">Она имеет простой цветовую шкалу и выбора «pop-down», которое предлагает возможности более точного управления.</span><span class="sxs-lookup"><span data-stu-id="19fa1-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="19fa1-237">Палитра содержит стандартную палитру цветов, поддерживает стандартные имена цветов, хэш-кодов, цвета RGB, RGBA, HSL и HSLA и ведет список цветов, которые наиболее недавно использовавшегося в документе.</span><span class="sxs-lookup"><span data-stu-id="19fa1-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="19fa1-238">В строке, где был в свойстве цвета фона введите «bc» и нажмите стрелку вниз один раз.</span><span class="sxs-lookup"><span data-stu-id="19fa1-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="19fa1-239">При вводе первый символ каждого слова в свойства, разделенных дефисом, такого как «background-color» технология IntelliSense отфильтровывает список для отображения только свойств, которые соответствуют:</span><span class="sxs-lookup"><span data-stu-id="19fa1-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Фильтрация IntelliSense значения](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="19fa1-241">Теперь введите двоеточие.</span><span class="sxs-lookup"><span data-stu-id="19fa1-241">Now type a colon.</span></span> <span data-ttu-id="19fa1-242">При этом вставляется имя свойства полный цвет фона.</span><span class="sxs-lookup"><span data-stu-id="19fa1-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="19fa1-243">Тип **#** или **rgb (**, и появляется палитра цветов:</span><span class="sxs-lookup"><span data-stu-id="19fa1-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Палитра цветов CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="19fa1-245">Чтобы увидеть, как работает средство выбора цветов, щелкните его цвета с указателем, или нажмите клавишу со стрелкой вниз и затем с помощью клавиш со стрелками влево и вправо для обхода цвета.</span><span class="sxs-lookup"><span data-stu-id="19fa1-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="19fa1-246">При посещении цвет, предварительном просмотре соответствующее значение для свойства цвет фона:</span><span class="sxs-lookup"><span data-stu-id="19fa1-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![значение свойства цвет фона предварительного просмотра](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="19fa1-248">На этом этапе можно нажать ВВОД, чтобы выбрать значение, а затем точку с запятой (;) для завершения записи CSS.</span><span class="sxs-lookup"><span data-stu-id="19fa1-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="19fa1-249">Сейчас перейдите к следующему разделу, чтобы можно было увидеть, как работает средство выбора цвета pop вниз.</span><span class="sxs-lookup"><span data-stu-id="19fa1-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="19fa1-250">С помощью средства выбора цвета Pop вниз</span><span class="sxs-lookup"><span data-stu-id="19fa1-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="19fa1-251">При цветовой полосы не имеет необходимого цвета, который вы ищете, можно использовать палитра pop вниз.</span><span class="sxs-lookup"><span data-stu-id="19fa1-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="19fa1-252">Чтобы открыть его, щелкните значок шеврона в правом конце цветовой полосы, или один или два раза нажмите клавишу со стрелкой вниз на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="19fa1-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Средство выбора цвета CSS, Pop вниз](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="19fa1-254">Выберите цвет из вертикальная черта справа.</span><span class="sxs-lookup"><span data-stu-id="19fa1-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="19fa1-255">Это показывает градиента цвета в главном окне.</span><span class="sxs-lookup"><span data-stu-id="19fa1-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="19fa1-256">Выбрать цвет непосредственно из вертикальной чертой, нажав клавишу ВВОД или щелкните любую точку в главном окне для выбора с большей точностью.</span><span class="sxs-lookup"><span data-stu-id="19fa1-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="19fa1-257">Есть ли цвет на экране компьютера, который вы хотите использовать (он не должен быть в пользовательском интерфейсе Visual Studio), его значение можно захватить, используя инструмент "Пипетка" в правом нижнем углу.</span><span class="sxs-lookup"><span data-stu-id="19fa1-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="19fa1-258">Также можно изменить непрозрачность цвета, переместив ползунок в нижней части палитры цветов.</span><span class="sxs-lookup"><span data-stu-id="19fa1-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="19fa1-259">Способ изменения значения RGBA значения цветов, так как формат RGBA может представлять непрозрачности.</span><span class="sxs-lookup"><span data-stu-id="19fa1-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="19fa1-260">После выбора цвета, нажмите клавишу ВВОД, а затем введите точку с запятой, чтобы завершить ввод цвет фона в *Site.css* файл.</span><span class="sxs-lookup"><span data-stu-id="19fa1-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="19fa1-261">Панель обновления инспектора страницы</span><span class="sxs-lookup"><span data-stu-id="19fa1-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="19fa1-262">Инспектор страниц немедленно обнаруживает изменение *Site.css* файл (или любой другой файл в приложении) и отображает предупреждение в панель обновления.</span><span class="sxs-lookup"><span data-stu-id="19fa1-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Панель обновления](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="19fa1-264">Чтобы сохранить все файлы и обновите содержимое браузера инспектора страниц, нажмите клавиши Ctrl + Alt + Ввод или щелкните панель обновления.</span><span class="sxs-lookup"><span data-stu-id="19fa1-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="19fa1-265">Изменение цвета выделения откроется в браузере:</span><span class="sxs-lookup"><span data-stu-id="19fa1-265">The change in the highlight color appears in the browser:</span></span>

![Изменить цвет выделения](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="19fa1-267">Обратите внимание, что удобно обновление браузера инспектора страниц прямо из среды Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fa1-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="19fa1-268">Использование инспектора страниц, а не во внешнем браузере позволяет оставаться в редакторе при разработке веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="19fa1-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="19fa1-269">См. также</span><span class="sxs-lookup"><span data-stu-id="19fa1-269">See Also</span></span>

<span data-ttu-id="19fa1-270">[Введение в инспектор страниц](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (видео Channel 9)</span><span class="sxs-lookup"><span data-stu-id="19fa1-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="19fa1-271">[Сообщения об ошибках инспектора страниц](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="19fa1-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
