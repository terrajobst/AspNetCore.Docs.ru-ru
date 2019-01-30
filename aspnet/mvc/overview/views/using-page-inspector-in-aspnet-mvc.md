---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Использование инспектора страниц в ASP.NET MVC | Документация Майкрософт
author: rick-anderson
description: Инспектор страниц в Visual Studio 2012 — это средство веб-разработки с интегрированным браузером. Выберите любой элемент в интегрированной браузером и инспектор страниц i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 0dea8b077878139a3f513cb51447b86a93fe55b8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236540"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="2f13f-104">Использование инспектора страниц в ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="2f13f-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="2f13f-105">Тим Ammann</span><span class="sxs-lookup"><span data-stu-id="2f13f-105">by Tim Ammann</span></span>

> <span data-ttu-id="2f13f-106">Инспектор страниц в Visual Studio 2012 — это средство веб-разработки с интегрированным браузером.</span><span class="sxs-lookup"><span data-stu-id="2f13f-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="2f13f-107">Выберите любой элемент в встроенного браузера и инспектор страниц мгновенно выделения источника этого элемента и CSS.</span><span class="sxs-lookup"><span data-stu-id="2f13f-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="2f13f-108">Можно просматривать любого представления MVC, быстро находить источники визуализированной разметке и использовать средства браузера непосредственно в среде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f13f-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="2f13f-109">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="2f13f-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="2f13f-110">Этом руководстве показано, как включить режим инспектирования и быстро найти и отредактировать разметку и CSS в веб-проект.</span><span class="sxs-lookup"><span data-stu-id="2f13f-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="2f13f-111">В этом руководстве используется проект MVC, но можно также использовать инспектор страниц для [веб-форм](https://go.microsoft.com/?linkid=9802001) и других приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f13f-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="2f13f-112">Учебник содержит следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="2f13f-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="2f13f-113">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="2f13f-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="2f13f-114">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="2f13f-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="2f13f-115">Использование инспектора страниц для обзора в представление</span><span class="sxs-lookup"><span data-stu-id="2f13f-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="2f13f-116">Включить режим проверки</span><span class="sxs-lookup"><span data-stu-id="2f13f-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="2f13f-117">Использование инспектора страниц вносить изменения в разметку</span><span class="sxs-lookup"><span data-stu-id="2f13f-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="2f13f-118">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="2f13f-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="2f13f-119">Просмотр изменений CSS в окне "Стили"</span><span class="sxs-lookup"><span data-stu-id="2f13f-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="2f13f-120">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="2f13f-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="2f13f-121">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="2f13f-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="2f13f-122">Сопоставление динамические элементы страниц JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f13f-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="2f13f-123">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="2f13f-123">Prerequisites</span></span>

- <span data-ttu-id="2f13f-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) или [Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="2f13f-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="2f13f-125">Чтобы получить последнюю версию инспектор страниц, используйте [установщика веб-платформы](https://go.microsoft.com/fwlink/?LinkId=255386) установке Windows Azure SDK для .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="2f13f-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="2f13f-126">Инспектор страниц входит в состав инструменты разработчика Microsoft Web.</span><span class="sxs-lookup"><span data-stu-id="2f13f-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="2f13f-127">Последняя версия — 1.3.</span><span class="sxs-lookup"><span data-stu-id="2f13f-127">The latest version is 1.3.</span></span> <span data-ttu-id="2f13f-128">Чтобы проверить, какая версия у, запустите Visual Studio и выберите **о Microsoft Visual Studio** из **помочь** меню.</span><span class="sxs-lookup"><span data-stu-id="2f13f-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="2f13f-129">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="2f13f-129">Create a Web Application</span></span>

<span data-ttu-id="2f13f-130">Во-первых создайте веб-приложения, который будет использоваться инспектор страниц с.</span><span class="sxs-lookup"><span data-stu-id="2f13f-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="2f13f-131">В Visual Studio, выберите **файл** &gt; **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="2f13f-132">Слева разверните **Visual C#** выберите **Web**, а затем выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Новое приложение ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="2f13f-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-134">Click **OK**.</span></span>

<span data-ttu-id="2f13f-135">В **создания проекта ASP.NET MVC 4** выберите **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="2f13f-136">Оставьте **Razor** как обработчик представлений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2f13f-136">Leave **Razor** as the default view engine.</span></span>

![Новый проект ASP.NET MVC — веб-приложение](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="2f13f-138">Приложение откроется в **источника** представления.</span><span class="sxs-lookup"><span data-stu-id="2f13f-138">The application opens in **Source** view.</span></span>

![Новое приложение ASP.NET MVC в представлении источника](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="2f13f-140">Теперь, когда у вас есть приложение для работы с, инспектор страниц можно использовать для проверки и изменения его.</span><span class="sxs-lookup"><span data-stu-id="2f13f-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="2f13f-141">Использование инспектора страниц для обзора в представление</span><span class="sxs-lookup"><span data-stu-id="2f13f-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="2f13f-142">В Visual Studio 2012, щелкнуть правой кнопкой мыши любого представления в проект и выберите **просмотреть в инспекторе страниц**, инспектор страниц будет и выяснить, маршрут на страницу.</span><span class="sxs-lookup"><span data-stu-id="2f13f-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="2f13f-143">В **обозревателе решений**, разверните **представления** папку и затем **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="2f13f-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="2f13f-144">Файл Index.cshtml щелкните правой кнопкой мыши и выберите **просмотреть в инспекторе страниц**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Просмотр в инспекторе страниц Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="2f13f-146">По умолчанию инспектор страниц расположенной в левой части среды Visual Studio как окно.</span><span class="sxs-lookup"><span data-stu-id="2f13f-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="2f13f-147">При желании можно закрепить его в другом месте или открепить окно.</span><span class="sxs-lookup"><span data-stu-id="2f13f-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="2f13f-148">См. практическое руководство по [ размещать и закреплять окна](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f13f-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="2f13f-149">В верхней области окна инспектора страниц показываются текущей страницы в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="2f13f-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="2f13f-150">На нижней панели отображаются страницы в разметке HTML, а также некоторые вкладки, которые позволяют проверять различные аспекты страницы.</span><span class="sxs-lookup"><span data-stu-id="2f13f-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="2f13f-151">На нижней панели аналогичен [средства разработчика F12](https://msdn.microsoft.com/ie/aa740478) в Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="2f13f-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Приложения ASP.NET MVC в инспекторе страниц](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="2f13f-153">В этом руководстве вы воспользуетесь **HTML** и **стили** вкладки для быстрого перехода и внесения изменений в приложение.</span><span class="sxs-lookup"><span data-stu-id="2f13f-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="2f13f-154">Режим EnableInspection</span><span class="sxs-lookup"><span data-stu-id="2f13f-154">EnableInspection Mode</span></span>

<span data-ttu-id="2f13f-155">Чтобы перевести инспектор страниц в режиме инспектирования, нажмите кнопку **инспектировать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="2f13f-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="2f13f-156">В режиме инспектирования Если навести указатель мыши на любую часть отображенной странице соответствующая исходная разметка или код будет выделен.</span><span class="sxs-lookup"><span data-stu-id="2f13f-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Переключиться в режим проверки](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="2f13f-158">Теперь наведите указатель мыши на различные части страницы в инспекторе страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="2f13f-159">После этого указатель мыши примет знака плюс, и выделяется под элементом:</span><span class="sxs-lookup"><span data-stu-id="2f13f-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![При наведении div.content оболочки](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="2f13f-161">При перемещении указателя мыши, Visual Studio выделяет соответствующий синтаксис Razor в исходном файле.</span><span class="sxs-lookup"><span data-stu-id="2f13f-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="2f13f-162">Если HTML-элемента берется из другого файла источника, Visual Studio автоматически открывает файл.</span><span class="sxs-lookup"><span data-stu-id="2f13f-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="2f13f-163">В инспекторе страниц **HTML** вкладке отображается HTML-код, который был создан из синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="2f13f-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="2f13f-164">При перемещении указателя мыши, выделены элементы HTML.</span><span class="sxs-lookup"><span data-stu-id="2f13f-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="2f13f-165">**Стили** вкладке отображаются правила CSS для элемента.</span><span class="sxs-lookup"><span data-stu-id="2f13f-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="2f13f-166">Использование инспектора страниц вносить изменения в разметку</span><span class="sxs-lookup"><span data-stu-id="2f13f-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="2f13f-167">Инспектор страниц позволяет найти разметки, местоположение которого могут быть неявными.</span><span class="sxs-lookup"><span data-stu-id="2f13f-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="2f13f-168">Затем можно изменить разметку и см. в разделе результирующих изменений.</span><span class="sxs-lookup"><span data-stu-id="2f13f-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="2f13f-169">Чтобы увидеть это, нажмите кнопку **инспектировать** и прокрутите до нижней части страницы в окне инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="2f13f-170">При перемещении указателя мыши в области нижнего колонтитула, инспектор страниц открывает \_файла Layout.cshtml и выделяет часть макета страницы, выбранный вами.</span><span class="sxs-lookup"><span data-stu-id="2f13f-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="2f13f-171">Как вы видите, нижний колонтитул, определенный в файле макета, а не само представление.</span><span class="sxs-lookup"><span data-stu-id="2f13f-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Нижние колонтитулы](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="2f13f-173">Теперь наведите указатель мыши на строку с авторских прав <a id="a"> </a>Обратите внимание, что.</span><span class="sxs-lookup"><span data-stu-id="2f13f-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="2f13f-174">В \_страницу Layout.cshtml, соответствующая строка будет выделена.</span><span class="sxs-lookup"><span data-stu-id="2f13f-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Строка нижнего колонтитула об авторских правах, выделяемая подсветкой](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="2f13f-176">Добавьте какой-нибудь текст в конец строки в \_файла Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2f13f-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="2f13f-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year -Мое приложение ASP.NET MVC крут хотите знать?! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="2f13f-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="2f13f-178">Теперь нажмите клавиши Ctrl + Alt + Ввод или щелкните панель обновления, чтобы просмотреть результаты в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="2f13f-180">Вы наверняка считали, что нижнего колонтитула, определенные в файле Index.cshtml. Однако это оказалось в \_Layout.cshtml и инспектор страниц нашли его для вас.</span><span class="sxs-lookup"><span data-stu-id="2f13f-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="2f13f-181">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="2f13f-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="2f13f-182">Затем вы получите краткий обзор HTML-окну, и способом сопоставления элементов для вас.</span><span class="sxs-lookup"><span data-stu-id="2f13f-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="2f13f-183">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="2f13f-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="2f13f-184">Щелкните в верхней части страницы «Вашей logohere».</span><span class="sxs-lookup"><span data-stu-id="2f13f-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="2f13f-185">Изучении конкретный элемент более подробно, поэтому отображение в окне браузера больше не изменяется при перемещении указателя мыши.</span><span class="sxs-lookup"><span data-stu-id="2f13f-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="2f13f-186">Теперь наведите указатель мыши на **HTML** окна.</span><span class="sxs-lookup"><span data-stu-id="2f13f-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="2f13f-187">При перемещении указателя мыши, инспектор страниц описаны сервисном **HTML** окне и выделяет соответствующий элемент в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="2f13f-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Окно HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="2f13f-189">Как и раньше, инспектор страниц открывает \_файла Layout.cshtml для вас в временную вкладку. Нажмите кнопку \_Layout.cshtml временную вкладку, а соответствующая разметка будет выделена &lt;заголовок&gt; раздел для вас:</span><span class="sxs-lookup"><span data-stu-id="2f13f-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Выделенная разметка](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="2f13f-191">Просмотр изменений CSS в окне "Стили"</span><span class="sxs-lookup"><span data-stu-id="2f13f-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="2f13f-192">Затем будет использовать инспектор страниц **стили** окно, чтобы просмотреть изменения в CSS.</span><span class="sxs-lookup"><span data-stu-id="2f13f-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="2f13f-193">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="2f13f-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="2f13f-194">В окне браузера инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** появится метка.</span><span class="sxs-lookup"><span data-stu-id="2f13f-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![При наведении div.content оболочки](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="2f13f-196">Щелкните в разделе div.content оболочку, а затем наведите указатель мыши на **стили** окна.</span><span class="sxs-lookup"><span data-stu-id="2f13f-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="2f13f-197">**Стили** окне показаны все правила CSS для данного элемента.</span><span class="sxs-lookup"><span data-stu-id="2f13f-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="2f13f-198">Прокрутите вниз до селектор класса поиска .featured .content оболочки.</span><span class="sxs-lookup"><span data-stu-id="2f13f-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="2f13f-199">Теперь снимите флажок для свойства цвет фона.</span><span class="sxs-lookup"><span data-stu-id="2f13f-199">Now clear the checkbox for the background-color property.</span></span>

![Цвет фона очистить](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="2f13f-201">Обратите внимание на то, как изменение выполняет предварительный просмотр сразу же в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="2f13f-202">Снова установите флажок, затем дважды щелкните значение свойства и измените его на красный.</span><span class="sxs-lookup"><span data-stu-id="2f13f-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="2f13f-203">Изменение немедленно показано:</span><span class="sxs-lookup"><span data-stu-id="2f13f-203">The change shows immediately:</span></span>

![Красного цвета фона](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="2f13f-205">**Стили** облегчает тестирование и предварительный просмотр CSS изменения, прежде чем зафиксировать изменения стиля делает окно листа сам.</span><span class="sxs-lookup"><span data-stu-id="2f13f-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="2f13f-206">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="2f13f-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="2f13f-207">Эта функция требует версии 1.3 инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="2f13f-208">Автосинхронизация CSS позволяет напрямую изменить CSS-файл и увидеть изменения прямо в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="2f13f-209">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="2f13f-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="2f13f-210">В браузере инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** появится метка.</span><span class="sxs-lookup"><span data-stu-id="2f13f-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="2f13f-211">Щелкните один раз, чтобы выбрать этот элемент.</span><span class="sxs-lookup"><span data-stu-id="2f13f-211">Click once to select this element.</span></span>

<span data-ttu-id="2f13f-212">**Стили** окне показаны все правила CSS для данного элемента.</span><span class="sxs-lookup"><span data-stu-id="2f13f-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="2f13f-213">Прокрутите вниз до селектор класса поиска .featured .content оболочки.</span><span class="sxs-lookup"><span data-stu-id="2f13f-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="2f13f-214">Щелкните «.featured .content оболочки».</span><span class="sxs-lookup"><span data-stu-id="2f13f-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="2f13f-215">Инспектор страниц открывает CSS-файл, который определяет этот стиль (Site.css) и выделяет соответствующий стиль CSS.</span><span class="sxs-lookup"><span data-stu-id="2f13f-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="2f13f-216">Теперь измените значение `background-color` «красный».</span><span class="sxs-lookup"><span data-stu-id="2f13f-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="2f13f-217">Изменение немедленно отображается в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="2f13f-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="2f13f-218">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="2f13f-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="2f13f-219">Редактор CSS в Visual Studio 2012 содержит палитру цветов, позволяет легко выбрать и вставить цвета.</span><span class="sxs-lookup"><span data-stu-id="2f13f-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="2f13f-220">Палитра содержит стандартную палитру цветов, поддерживает стандартные имена цветов, хэш-кодов, цвета RGB, RGBA, HSL и HSLA и ведет список цветов, которые наиболее недавно использовавшегося в документе.</span><span class="sxs-lookup"><span data-stu-id="2f13f-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="2f13f-221">В предыдущем разделе, вы изменили значение `background-color` свойства.</span><span class="sxs-lookup"><span data-stu-id="2f13f-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="2f13f-222">Чтобы вызвать палитра цветов, поместите курсор после имени свойства и типа **#** или **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Палитра цветов CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="2f13f-224">Щелкните цвет, чтобы выбрать его, или нажмите клавишу со стрелкой вниз и затем с помощью клавиш со стрелками влево и вправо для обхода цвета.</span><span class="sxs-lookup"><span data-stu-id="2f13f-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="2f13f-225">При посещении цвет, предварительном просмотре соответствующее шестнадцатеричное значение:</span><span class="sxs-lookup"><span data-stu-id="2f13f-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![значение свойства цвет фона предварительного просмотра](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="2f13f-227">Если цветовой полосы не имеет точного нужного цвета, можно использовать средство выбора цвета pop вниз.</span><span class="sxs-lookup"><span data-stu-id="2f13f-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="2f13f-228">Чтобы открыть его, щелкните значок шеврона в правом конце цветовой полосы, или один или два раза нажмите клавишу со стрелкой вниз на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="2f13f-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Средство выбора цвета CSS, Pop вниз](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="2f13f-230">Выберите цвет из вертикальная черта справа.</span><span class="sxs-lookup"><span data-stu-id="2f13f-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="2f13f-231">Это показывает градиента цвета в главном окне.</span><span class="sxs-lookup"><span data-stu-id="2f13f-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="2f13f-232">Выбрать цвет непосредственно из вертикальной чертой, нажав клавишу ВВОД или щелкните любую точку в главном окне для выбора с большей точностью.</span><span class="sxs-lookup"><span data-stu-id="2f13f-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="2f13f-233">Есть ли цвет на экране компьютера, который вы хотите использовать (он не должен быть в пользовательском интерфейсе Visual Studio), его значение можно захватить, используя инструмент "Пипетка" в правом нижнем углу.</span><span class="sxs-lookup"><span data-stu-id="2f13f-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="2f13f-234">Также можно изменить непрозрачность цвета, переместив ползунок в нижней части палитры цветов.</span><span class="sxs-lookup"><span data-stu-id="2f13f-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="2f13f-235">Способ изменения цвета значения в значения RGBA, так как формат RGBA может представлять непрозрачности.</span><span class="sxs-lookup"><span data-stu-id="2f13f-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="2f13f-236">После выбора цвета, нажмите клавишу ВВОД, а затем введите точку с запятой, чтобы завершить ввод цвет фона в *Site.css* файл.</span><span class="sxs-lookup"><span data-stu-id="2f13f-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="2f13f-237">Панель обновления инспектора страницы</span><span class="sxs-lookup"><span data-stu-id="2f13f-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="2f13f-238">Инспектор страниц немедленно обнаруживает изменение *Site.css* файла и отображает предупреждение в панель обновления.</span><span class="sxs-lookup"><span data-stu-id="2f13f-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Панель обновления](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="2f13f-240">Чтобы сохранить все файлы и обновите содержимое браузера инспектора страниц, нажмите клавиши Ctrl + Alt + Ввод или щелкните панель обновления.</span><span class="sxs-lookup"><span data-stu-id="2f13f-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="2f13f-241">Изменение цвета выделения откроется в браузере.</span><span class="sxs-lookup"><span data-stu-id="2f13f-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="2f13f-242">Сопоставление динамические элементы страниц JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f13f-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="2f13f-243">В современных веб-приложений элементы страницы часто создается динамически с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f13f-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="2f13f-244">Это означает, что статические разметка отсутствует (HTML или Razor), соответствующий эти элементы страницы.</span><span class="sxs-lookup"><span data-stu-id="2f13f-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="2f13f-245">С версии 1.3 инспектор страниц теперь можно изменять элементы, которые были динамически добавлены на страницу обратно на соответствующий код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f13f-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="2f13f-246">Чтобы продемонстрировать эту функцию, мы будем использовать [одностраничное приложение (SPA) шаблона](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="2f13f-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2f13f-247">Шаблон одностраничного приложения требуется [ASP.NET и веб-инструментами 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) обновления.</span><span class="sxs-lookup"><span data-stu-id="2f13f-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="2f13f-248">В Visual Studio, выберите **файл** &gt; **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="2f13f-249">Слева разверните **Visual C#** выберите **Web**, а затем выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="2f13f-250">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-250">Click **OK**.</span></span>

<span data-ttu-id="2f13f-251">В **создания проекта ASP.NET MVC 4** диалоговом окне выберите **одностраничного приложения**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="2f13f-252">В обозревателе решений разверните **представления** папку и затем **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="2f13f-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="2f13f-253">Файл Index.cshtml щелкните правой кнопкой мыши и выберите **просмотреть в инспекторе страниц**.</span><span class="sxs-lookup"><span data-stu-id="2f13f-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="2f13f-254">Первое, что это отображается в браузере инспектора страниц — это страница входа.</span><span class="sxs-lookup"><span data-stu-id="2f13f-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="2f13f-255">Щелкните «Зарегистрироваться» и создайте имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="2f13f-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="2f13f-256">После регистрации, приложение вход будет выполнен и создает список дел с некоторых образцов элементов.</span><span class="sxs-lookup"><span data-stu-id="2f13f-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="2f13f-257">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме инспектирования.</span><span class="sxs-lookup"><span data-stu-id="2f13f-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="2f13f-258">В браузере инспектора страниц щелкните один из элементов списка дел.</span><span class="sxs-lookup"><span data-stu-id="2f13f-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="2f13f-259">Обратите внимание на то, что вместо выделены синим цветом, элемент выделены оранжевым цветом, с помощью «JS» рядом с именем элемента.</span><span class="sxs-lookup"><span data-stu-id="2f13f-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="2f13f-260">Это означает, что элемент был создан динамически при помощи сценария.</span><span class="sxs-lookup"><span data-stu-id="2f13f-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="2f13f-261">Кроме того, оранжевый подчеркивание отображается на **стек вызовов** вкладки. Это означает, что **стек вызовов** панели находятся дополнительные сведения об элементе.</span><span class="sxs-lookup"><span data-stu-id="2f13f-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="2f13f-262">Щелкните **стек вызовов** вкладки. **Стек вызовов** области показан стек вызовов для вызова JavaScript, создавшего элемент.</span><span class="sxs-lookup"><span data-stu-id="2f13f-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="2f13f-263">Вызывается на внешние библиотеки такие, как jQuery свернуты, можно легко увидеть вызовы к сценарию приложения.</span><span class="sxs-lookup"><span data-stu-id="2f13f-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="2f13f-264">Чтобы просмотреть полный стек, включая вызовы на внешние библиотеки можно развернуть узлы с меткой «Внешние библиотеки»:</span><span class="sxs-lookup"><span data-stu-id="2f13f-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="2f13f-265">Если щелкнуть элемент в стеке вызовов, Visual Studio открывает файл кода и выделяет соответствующий сценарий.</span><span class="sxs-lookup"><span data-stu-id="2f13f-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="2f13f-266">См. также</span><span class="sxs-lookup"><span data-stu-id="2f13f-266">See Also</span></span>

<span data-ttu-id="2f13f-267">[Введение в ASP.NET MVC 4 с помощью Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (веб-сайт ASP.net)</span><span class="sxs-lookup"><span data-stu-id="2f13f-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="2f13f-268">[Введение в инспектор страниц](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (видео Channel 9)</span><span class="sxs-lookup"><span data-stu-id="2f13f-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="2f13f-269">[Сообщения об ошибках инспектора страниц](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="2f13f-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
