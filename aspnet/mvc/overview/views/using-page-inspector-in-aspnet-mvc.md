---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "С помощью инспектора страниц в ASP.NET MVC | Документы Microsoft"
author: rick-anderson
description: "Инспектор страниц в Visual Studio 2012 — средство веб-разработки с интегрированным браузером. Выберите любой элемент в встроенном браузере и инспектор страниц i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="cbbc2-104">Использование инспектора страниц в ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cbbc2-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="cbbc2-105">по Тим Ammann</span><span class="sxs-lookup"><span data-stu-id="cbbc2-105">by Tim Ammann</span></span>

> <span data-ttu-id="cbbc2-106">Инспектор страниц в Visual Studio 2012 — средство веб-разработки с интегрированным браузером.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="cbbc2-107">Выберите любой элемент во встроенном браузере и инспектор страниц мгновенно выделяет исходного элемента и CSS.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="cbbc2-108">Можно просматривать любого представления MVC, быстро найти источники для отображения разметки и использовать средства браузера непосредственно в среде Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="cbbc2-109">В видео</span><span class="sxs-lookup"><span data-stu-id="cbbc2-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="cbbc2-110">Этого учебника показано, как включить режим инспектирования и быстро найти и отредактировать разметки и CSS веб-проекта.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="cbbc2-111">В этом учебнике используется проект MVC, но можно также использовать инспектор страниц для [Web Forms](https://go.microsoft.com/?linkid=9802001) и других приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="cbbc2-112">Учебник содержит следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="cbbc2-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="cbbc2-113">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="cbbc2-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="cbbc2-114">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="cbbc2-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="cbbc2-115">Использовать инспектор страниц в перейдите к представлению</span><span class="sxs-lookup"><span data-stu-id="cbbc2-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="cbbc2-116">Включить режим инспектирования</span><span class="sxs-lookup"><span data-stu-id="cbbc2-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="cbbc2-117">Для внесения изменений в разметке использовать инспектор страниц</span><span class="sxs-lookup"><span data-stu-id="cbbc2-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="cbbc2-118">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="cbbc2-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="cbbc2-119">Просмотр изменений CSS в окне «Стили»</span><span class="sxs-lookup"><span data-stu-id="cbbc2-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="cbbc2-120">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="cbbc2-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="cbbc2-121">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="cbbc2-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="cbbc2-122">Сопоставление динамические элементы страниц в код JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbbc2-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="cbbc2-123">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cbbc2-123">Prerequisites</span></span>

- <span data-ttu-id="cbbc2-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) или [Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="cbbc2-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="cbbc2-125">Чтобы получить последнюю версию инспектор страниц, используйте [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) установке Windows Azure SDK для .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="cbbc2-126">Инспектор страниц входит в состав средств веб-разработки Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="cbbc2-127">Последняя версия — 1.3.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-127">The latest version is 1.3.</span></span> <span data-ttu-id="cbbc2-128">Проверка версии имеют, запустите Visual Studio и выберите **о Microsoft Visual Studio** из **справки** меню.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="cbbc2-129">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="cbbc2-129">Create a Web Application</span></span>

<span data-ttu-id="cbbc2-130">Во-первых создайте веб-приложения, который будет использоваться инспектор страниц с.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="cbbc2-131">В Visual Studio выберите **файл** &gt; **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="cbbc2-132">Слева разверните узел **Visual C#**выберите **Web**, а затем выберите **веб-приложение ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Новое приложение ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="cbbc2-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-134">Click **OK**.</span></span>

<span data-ttu-id="cbbc2-135">В **нового проекта ASP.NET MVC 4** выберите **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="cbbc2-136">Оставить **Razor** как обработчик представлений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-136">Leave **Razor** as the default view engine.</span></span>

![Новый проект ASP.NET MVC - приложений в Интернете](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="cbbc2-138">Приложение откроется в **источника** представления.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-138">The application opens in **Source** view.</span></span>

![Новое приложение ASP.NET MVC в представлении исходного кода](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="cbbc2-140">Теперь, когда приложение для работы с инспектор страниц можно использовать для проверки и изменения его.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="cbbc2-141">Использовать инспектор страниц в перейдите к представлению</span><span class="sxs-lookup"><span data-stu-id="cbbc2-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="cbbc2-142">В Visual Studio 2012, щелкнуть правой кнопкой мыши любое представление в своем проекте выберите **просмотреть в инспекторе страниц**, инспектор страниц будет выяснить, маршрута и отображения страницы.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="cbbc2-143">В **обозревателе решений**, разверните **представления** папки и затем **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="cbbc2-144">Щелкните правой кнопкой мыши файл Index.cshtml и выберите **просмотреть в инспекторе страниц**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Index.cshtml Просмотр в инспекторе страниц](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="cbbc2-146">По умолчанию инспектор страниц закреплен в окне слева от среды Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="cbbc2-147">При желании можно закрепить его в другом месте или открепить окно.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="cbbc2-148">В разделе [как: размещение и закрепление окон](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="cbbc2-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="cbbc2-149">В верхней области окна инспектора страниц показываются текущей страницы в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="cbbc2-150">На нижней панели отображаются страницы в разметке HTML, а также некоторые вкладки, где можно просмотреть различные аспекты страницы.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="cbbc2-151">На нижней панели аналогичен [средств разработчика F12](https://msdn.microsoft.com/ie/aa740478) в Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Приложение ASP.NET MVC в инспекторе страниц](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="cbbc2-153">В этом учебнике используется **HTML** и **стили** вкладки для быстрого перехода и внесения изменений в приложение.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="cbbc2-154">Режим EnableInspection</span><span class="sxs-lookup"><span data-stu-id="cbbc2-154">EnableInspection Mode</span></span>

<span data-ttu-id="cbbc2-155">Чтобы перевести инспектор страниц в режиме инспектирования, нажмите кнопку **инспектировать** кнопки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="cbbc2-156">В режиме инспектирования когда указатель мыши над любой частью подготовленной странице соответствующая разметка исходного кода выделяется.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Переключиться в режим проверки](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="cbbc2-158">Теперь наведите курсор на различные части страницы в инспекторе страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="cbbc2-159">После этого указатель мыши изменяется знака плюс и будет выделен под элементом:</span><span class="sxs-lookup"><span data-stu-id="cbbc2-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Наведении указателя мыши на div.content оболочки](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="cbbc2-161">При перемещении указателя мыши в Visual Studio выделяет соответствующий синтаксис Razor в исходном файле.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="cbbc2-162">Если HTML-элемент находится на другом исходном файле, Visual Studio автоматически открывает файл.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="cbbc2-163">В инспекторе страниц **HTML** вкладке отображается HTML-код, который был создан с помощью синтаксиса Razor.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="cbbc2-164">При перемещении указателя мыши, выделяются HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="cbbc2-165">**Стили** вкладке отображаются правила CSS для элемента.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="cbbc2-166">Для внесения изменений в разметке использовать инспектор страниц</span><span class="sxs-lookup"><span data-stu-id="cbbc2-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="cbbc2-167">Инспектор страниц позволяет найти разметки, расположение которого могут быть неявными.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="cbbc2-168">Затем можно изменить разметку и результирующие изменения.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="cbbc2-169">Чтобы увидеть его, нажмите кнопку **инспектировать** и прокрутите до нижней части страницы в окне инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="cbbc2-170">При перемещении указателя мыши в области нижнего колонтитула инспектор страниц будет открывать \_Layout.cshtml файл и выделяет раздел страницы макета, выбранный вами.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="cbbc2-171">Как видите, нижний колонтитул, определенные в файле макета, а не самого представления.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Нижние колонтитулы](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="cbbc2-173">Теперь наведите указатель мыши на строку с авторских прав <a id="a"> </a>Обратите внимание.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="cbbc2-174">В \_страницу Layout.cshtml, соответствующая строка будет выделена.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Выделенной об авторских правах строке нижнего колонтитула](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="cbbc2-176">Добавьте в конец строки в текст \_Layout.cshtml файл.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="cbbc2-177">&lt;p&gt;&amp;копирование; @DateTime.Now.Year -Rocks Мое приложение ASP.NET MVC! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="cbbc2-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="cbbc2-178">Сейчас нажмите клавиши Ctrl + Alt + Ввод или щелкните панель обновления, чтобы просмотреть результаты в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Мои Rocks приложения ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="cbbc2-180">Вы могли подумать нижнего колонтитула, определенные в Index.cshtml, что она отключена в \_Layout.cshtml и найти он автоматически инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="cbbc2-181">Режим инспектирования и окна HTML</span><span class="sxs-lookup"><span data-stu-id="cbbc2-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="cbbc2-182">Затем необходимо будет кратко рассмотрим окна HTML и как они соответствуют элементы для вас.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="cbbc2-183">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="cbbc2-184">Нажмите кнопку в верхней части страницы с надписью «Вашей logohere».</span><span class="sxs-lookup"><span data-stu-id="cbbc2-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="cbbc2-185">Просматриваемую конкретный элемент более подробно, поэтому отображения в окне браузера больше не изменяется при перемещении указателя мыши.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="cbbc2-186">Теперь наведите указатель мыши на **HTML** окна.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="cbbc2-187">При перемещении указателя мыши инспектор страниц описаны элемент внутри **HTML** окна и выделяет соответствующий элемент в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Окно HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="cbbc2-189">Как и прежде, инспектор страниц будет открывать \_Layout.cshtml файл на временной вкладке. Нажмите кнопку \_Layout.cshtml временные вкладку и соответствующая разметка будет выделен на &lt;заголовок&gt; раздел для вас:</span><span class="sxs-lookup"><span data-stu-id="cbbc2-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Выделенная разметки](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="cbbc2-191">Просмотр изменений CSS в окне «Стили»</span><span class="sxs-lookup"><span data-stu-id="cbbc2-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="cbbc2-192">Далее можно использовать инспектор страниц **стили** окно для предварительного просмотра изменения CSS.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="cbbc2-193">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="cbbc2-194">В окне браузера инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** метка отображается.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Наведении указателя мыши на div.content оболочки](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="cbbc2-196">Щелкните в разделе div.content оболочку, а затем наведите указатель мыши на **стили** окна.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="cbbc2-197">**Syles** окне показаны все правила CSS для этого элемента.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="cbbc2-198">Прокрутите вниз до селектор класса поиска .featured .content оболочки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="cbbc2-199">Теперь снимите флажок для свойства цвет фона.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-199">Now clear the checkbox for the background-color property.</span></span>

![Цвет фона, снимите флажок](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="cbbc2-201">Обратите внимание на то, как изменение Предварительный просмотр мгновенно в окне браузера инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="cbbc2-202">Снова установите флажок, затем дважды щелкните значение свойства и измените его на красный.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="cbbc2-203">Изменения немедленно показано:</span><span class="sxs-lookup"><span data-stu-id="cbbc2-203">The change shows immediately:</span></span>

![Красного цвета фона](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="cbbc2-205">**Стили** облегчает тестирование и предварительного просмотра CSS изменяется до фиксации изменений стиль делает окно листа сам.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="cbbc2-206">Автосинхронизация CSS</span><span class="sxs-lookup"><span data-stu-id="cbbc2-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="cbbc2-207">Этот компонент требуется версия 1.3 инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="cbbc2-208">Функция автоматической синхронизации CSS позволяет напрямую изменить CSS-файл и увидеть изменения непосредственно в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="cbbc2-209">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="cbbc2-210">В браузере инспектора страниц наведите указатель мыши на раздел «Домашняя страница» до **div.content оболочки** метка отображается.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="cbbc2-211">Щелкните один раз, чтобы выбрать этот элемент.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-211">Click once to select this element.</span></span>

<span data-ttu-id="cbbc2-212">**Syles** окне показаны все правила CSS для этого элемента.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="cbbc2-213">Прокрутите вниз до селектор класса поиска .featured .content оболочки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="cbbc2-214">Щелкните «.featured .content-программу-оболочку».</span><span class="sxs-lookup"><span data-stu-id="cbbc2-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="cbbc2-215">Инспектор страниц открывает CSS-файл, определяющий этот стиль (Site.css) и выделяет соответствующего стиля CSS.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="cbbc2-216">Теперь измените значение `background-color` для «red».</span><span class="sxs-lookup"><span data-stu-id="cbbc2-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="cbbc2-217">Изменение немедленно отображается в браузере инспектора страниц.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="cbbc2-218">В палитре цветов CSS</span><span class="sxs-lookup"><span data-stu-id="cbbc2-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="cbbc2-219">Редактор CSS в Visual Studio 2012 имеет палитру цветов, позволяющие легко выбрать и вставить цвета.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="cbbc2-220">Палитра содержит стандартную палитру цветов и поддерживает стандартные имена цветов, хэш-кодов, цветов RGB, RGBA, HSL и HSLA ведет список цветов, которые вы недавно использовали в документе.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="cbbc2-221">В предыдущем разделе, вы изменили значение `background-color` свойства.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="cbbc2-222">Чтобы вызвать палитру цветов, поместите курсор после имени свойства и типа  **#**  или **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Палитра цветов CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="cbbc2-224">Щелкните цвет, выберите его и нажмите клавишу Стрелка вниз, а затем используйте клавиши со стрелками влево и вправо для перехода цвета.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="cbbc2-225">При посещении цвет, соответствующий шестнадцатеричное значение открывается для предварительного просмотра:</span><span class="sxs-lookup"><span data-stu-id="cbbc2-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![значение свойства цвет фона предварительного просмотра](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="cbbc2-227">Если цветовой шкалы не имеет точного нужного цвета, можно использовать средство выбора цвета pop раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="cbbc2-228">Чтобы открыть его, щелкните значок шеврона в правом конце цветовой шкалы или один или два раза нажмите клавишу Стрелка вниз на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Палитра цветов CSS Pop раскрывающегося списка](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="cbbc2-230">Выберите цвет из вертикальная черта справа.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="cbbc2-231">Здесь показан градиент цвета в главном окне.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="cbbc2-232">Выберите цвет непосредственно на вертикальную панель, нажав клавишу ВВОД или щелкнув любую из точек в главном окне выберите с большей точностью.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="cbbc2-233">Существует ли цвет на экране компьютера, который вы хотите использовать (это не обязательно должно находиться в пользовательском интерфейсе Visual Studio), его значение можно записать с помощью пипетки в правом нижнем углу.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="cbbc2-234">Также можно изменить степени непрозрачности цвета, переместив ползунок в нижней части палитру цветов.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="cbbc2-235">Таким образом, изменения цвета значения RGBA, так как формат RGBA может представлять непрозрачности.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="cbbc2-236">После выбора цвета, нажмите клавишу ВВОД, а затем введите точку с запятой для завершения операции цвет фона в *Site.css* файла.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="cbbc2-237">Панель обновления инспектора страницы</span><span class="sxs-lookup"><span data-stu-id="cbbc2-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="cbbc2-238">Инспектор страниц немедленно обнаруживает изменение *Site.css* и отображает предупреждение в панель обновления.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Панель обновления](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="cbbc2-240">Чтобы сохранить все файлы и обновить страницу в браузере инспектора страниц, нажмите сочетание клавиш Ctrl + Alt + Ввод или щелкните панель обновления.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="cbbc2-241">Изменение цвета выделения откроется в браузере.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="cbbc2-242">Сопоставление динамические элементы страниц в код JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbbc2-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="cbbc2-243">В современных веб-приложений элементы на странице часто создаются динамически с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="cbbc2-244">Это означает, что имеется не разметки static (HTML или Razor), соответствующий эти элементы страницы.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="cbbc2-245">С версии 1.3 инспектор страниц теперь можно изменять элементы, добавленные динамически на страницу обратно в соответствующий код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="cbbc2-246">Чтобы продемонстрировать эту функцию, мы будем использовать [шаблона приложения одной странице (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="cbbc2-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cbbc2-247">Требуется шаблон SPA [ASP.NET и 2012.2 средства Web](https://go.microsoft.com/fwlink/?LinkId=282650) обновления.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="cbbc2-248">В Visual Studio выберите **файл** &gt; **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="cbbc2-249">Слева разверните узел **Visual C#**выберите **Web**, а затем выберите **веб-приложение ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="cbbc2-250">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-250">Click **OK**.</span></span>

<span data-ttu-id="cbbc2-251">В **нового проекта ASP.NET MVC 4** диалогового окна выберите **одностраничного приложения**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="cbbc2-252">В обозревателе решений откройте **представления** папки и затем **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="cbbc2-253">Щелкните правой кнопкой мыши файл Index.cshtml и выберите **просмотреть в инспекторе страниц**.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="cbbc2-254">Первое, что именно отображается в браузере инспектора страниц — это страница входа в систему.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="cbbc2-255">Нажмите кнопку «Зарегистрироваться» и создайте имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="cbbc2-256">После регистрации, вход в приложение и создает список дел с некоторых элементов.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="cbbc2-257">Нажмите кнопку **инспектировать** чтобы перевести инспектор страниц в режиме проверки.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="cbbc2-258">В браузере инспектора страниц щелкните один из элементов, задачу.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="cbbc2-259">Обратите внимание, что вместо выделены синим цветом, элемент выделен оранжевым цветом, с «JS» рядом с именем элемента.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="cbbc2-260">Это означает, что элемент был создан динамически при помощи сценария.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="cbbc2-261">Кроме того, оранжевый подчеркивание отображается на **стек вызовов** вкладки. Это означает, что **стек вызовов** панель содержит дополнительные сведения об элементе.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="cbbc2-262">Щелкните **стек вызовов** вкладки. **Стек вызовов** области отображается стек вызовов для вызова JavaScript, создавшего элемент.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="cbbc2-263">Вызывается на внешние библиотеки, например jQuery свернуты, легко увидеть вызовы к сценарию приложения.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="cbbc2-264">Чтобы просмотреть полный стек, включая вызовы внешние библиотеки, можно развернуть в узлах «Внешние библиотеки»:</span><span class="sxs-lookup"><span data-stu-id="cbbc2-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="cbbc2-265">Если щелкнуть элемент в стеке вызовов, Visual Studio открывает файл кода и выделяет соответствующих сценариев.</span><span class="sxs-lookup"><span data-stu-id="cbbc2-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="cbbc2-266">См. также</span><span class="sxs-lookup"><span data-stu-id="cbbc2-266">See Also</span></span>

<span data-ttu-id="cbbc2-267">[Введение в ASP.NET MVC 4 с помощью Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (веб-сайта ASP.net)</span><span class="sxs-lookup"><span data-stu-id="cbbc2-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="cbbc2-268">[Введение в инспектор страниц](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (видео на канале 9)</span><span class="sxs-lookup"><span data-stu-id="cbbc2-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="cbbc2-269">[Сообщения об ошибках инспектора страниц](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="cbbc2-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
