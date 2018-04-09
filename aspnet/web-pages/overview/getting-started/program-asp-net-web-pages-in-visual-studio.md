---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio | Документы Microsoft
author: tfitzmac
description: В этом приложении объясняется, как можно использовать Visual Studio 2010 или Visual Web Developer 2010 Express программу веб-страниц ASP.NET с синтаксисом Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="4f77e-103">Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f77e-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="4f77e-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4f77e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4f77e-105">В этой статье объясняется, как можно использовать Visual Studio или Visual Web Developer Express программу веб-сайтов ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="4f77e-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="4f77e-106">Вы узнаете</span><span class="sxs-lookup"><span data-stu-id="4f77e-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="4f77e-107">Что необходимо для установки (при ее наличии) для работы с веб-страниц ASP.NET в вашей версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f77e-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="4f77e-108">Как добавить поддержку для веб-страниц ASP.NET в Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="4f77e-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="4f77e-109">Способы использования возможностей в Visual Studio для работы с страниц ASP.NET Razor, включая IntelliSense и отладчик.</span><span class="sxs-lookup"><span data-stu-id="4f77e-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4f77e-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="4f77e-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4f77e-111">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="4f77e-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="4f77e-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4f77e-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="4f77e-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="4f77e-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="4f77e-114">Этот учебник также работает с 2 веб-страниц ASP.NET, Visual Studio 2012, Visual Studio 2010 и WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="4f77e-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="4f77e-115">Можно создавать веб-страницы ASP.NET с синтаксисом Razor с помощью WebMatrix или другие редакторы кода.</span><span class="sxs-lookup"><span data-stu-id="4f77e-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="4f77e-116">Также можно использовать Microsoft Visual Studio, которая является полнофункциональным интегрированная среда разработки (IDE), предоставляет мощный набор средств для создания многих типов приложений (не только веб-сайтов).</span><span class="sxs-lookup"><span data-stu-id="4f77e-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="4f77e-117">Для работы с ASP.NET Razor страниц, можно установить несколько полных выпусках Visual Studio либо бесплатный [Visual Studio Express для Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) выпуска.</span><span class="sxs-lookup"><span data-stu-id="4f77e-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="4f77e-118">Две функции особенно полезна, предоставляемых Visual Studio для программирования веб-страниц ASP.NET Razor</span><span class="sxs-lookup"><span data-stu-id="4f77e-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="4f77e-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="4f77e-119">*IntelliSense*.</span></span> <span data-ttu-id="4f77e-120">Функции IntelliSense, встроенные в Visual Studio является мощным, чем технология IntelliSense в WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4f77e-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="4f77e-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="4f77e-121">*Debugger*.</span></span> <span data-ttu-id="4f77e-122">Отладчик позволяет устранить путем остановки программы, пока он работает, проверки переменных и пошаговое выполнение кода по одной строке кода.</span><span class="sxs-lookup"><span data-stu-id="4f77e-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="4f77e-123">С помощью Visual Studio с разными версиями веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f77e-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="4f77e-124">Visual Studio 2012 и Visual Studio 2013, включают поддержку для веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f77e-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="4f77e-125">(Пакеты, которые требуются для поддержки веб-страниц ASP.NET устанавливаются при установке Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="4f77e-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="4f77e-126">Visual Studio 2010 не поддерживает по умолчанию для веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f77e-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="4f77e-127">Чтобы использовать веб-страниц ASP.NET с помощью Visual Studio 2010, необходимо установить пакет ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f77e-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="4f77e-128">Для доступа к веб-страницы ASP.NET 2 установите ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="4f77e-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="4f77e-129">В следующей таблице перечислены поддержка для веб-страниц ASP.NET в различных версиях Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f77e-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="4f77e-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="4f77e-130">Visual Studio 2010</span></span> | <span data-ttu-id="4f77e-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4f77e-131">Visual Studio 2012</span></span> | <span data-ttu-id="4f77e-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4f77e-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4f77e-133">**Веб-страницы ASP.NET 2**</span><span class="sxs-lookup"><span data-stu-id="4f77e-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="4f77e-134">Установка ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="4f77e-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="4f77e-135">(Включено)</span><span class="sxs-lookup"><span data-stu-id="4f77e-135">(Included)</span></span> | <span data-ttu-id="4f77e-136">(Включено)</span><span class="sxs-lookup"><span data-stu-id="4f77e-136">(Included)</span></span> |
| <span data-ttu-id="4f77e-137">**Веб-страницы ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="4f77e-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="4f77e-138">Обновление для ASP.NET Web Pages 3 с помощью NuGet</span><span class="sxs-lookup"><span data-stu-id="4f77e-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="4f77e-139">(Включено)</span><span class="sxs-lookup"><span data-stu-id="4f77e-139">(Included)</span></span> |

<span data-ttu-id="4f77e-140">Для работы с Visual Studio 2010, в разделе [Установка поддержки для веб-страниц ASP.NET в Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="4f77e-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="4f77e-141">При запуске Visual Studio из WebMatrix</span><span class="sxs-lookup"><span data-stu-id="4f77e-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="4f77e-142">Если вы запустили проект в WebMatrix и переключитесь в Visual Studio, WebMatrix содержит кнопку, чтобы легко открыть проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f77e-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="4f77e-143">Необходимо иметь Visual Studio, установленной на компьютере для этой кнопки должно быть включено.</span><span class="sxs-lookup"><span data-stu-id="4f77e-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="4f77e-144">На следующем изображении показана кнопки в WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4f77e-144">The following image shows the button in WebMatrix.</span></span>

![Запустите Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="4f77e-146">При нажатии кнопки проект открыт в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f77e-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="4f77e-147">Вы можно переключаться между WebMatrix и Visual Studio без проблем.</span><span class="sxs-lookup"><span data-stu-id="4f77e-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="4f77e-148">Вы получите уведомление, если все файлы были изменены в другой среде и необходимо перезагрузить для получения последних изменений.</span><span class="sxs-lookup"><span data-stu-id="4f77e-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="4f77e-149">Создание сайта ASP.NET Razor в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f77e-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="4f77e-150">Чтобы создать на веб-сайт ASP.NET Razor в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4f77e-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="4f77e-151">Запустите Visual Studio или Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="4f77e-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="4f77e-152">В **файл** меню, нажмите кнопку **новый веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="4f77e-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Создание нового веб-сайта](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="4f77e-154">В **новый веб-сайт** диалогового окна выберите используемый язык (Visual C# или Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="4f77e-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="4f77e-155">Выберите **веб-сайт ASP.NET (Razor)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="4f77e-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![узел Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="4f77e-157">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4f77e-157">Click **OK**.</span></span>

<span data-ttu-id="4f77e-158">Новый проект существует и не будет заполнен некоторые веб-страницы по умолчанию, чтобы помочь вам приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="4f77e-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="4f77e-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="4f77e-159">Using IntelliSense</span></span>

<span data-ttu-id="4f77e-160">После создания сайта, вы увидите, как работает IntelliSense в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f77e-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="4f77e-161">Откройте в веб-сайт, вы только что создали, *Default.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="4f77e-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="4f77e-162">После `<h3>` теги на странице введите `@ServerInfo.` (включая точку).</span><span class="sxs-lookup"><span data-stu-id="4f77e-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="4f77e-163">Обратите внимание на то, как IntelliSense отображает доступные методы `ServerInfo` вспомогательный в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="4f77e-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="4f77e-165">Выберите `GetHtml` метод из списка и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="4f77e-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="4f77e-166">IntelliSense автоматически заполняет метод.</span><span class="sxs-lookup"><span data-stu-id="4f77e-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="4f77e-167">(Как с любым методом в C# необходимо добавить `()` символов после метода.)</span><span class="sxs-lookup"><span data-stu-id="4f77e-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
   <span data-ttu-id="4f77e-168">Полный код `GetHtml` метод выглядит как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="4f77e-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="4f77e-169">Нажмите клавиши Ctrl + F5, чтобы запустить страницу.</span><span class="sxs-lookup"><span data-stu-id="4f77e-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="4f77e-170">Это выглядит страницы при отображению в браузере:</span><span class="sxs-lookup"><span data-stu-id="4f77e-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![страница по умолчанию в браузере](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="4f77e-172">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="4f77e-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="4f77e-173">С помощью отладчика</span><span class="sxs-lookup"><span data-stu-id="4f77e-173">Using the Debugger</span></span>

1. <span data-ttu-id="4f77e-174">В верхней части *Default.cshtml* страницы после строки, которая начинается с `Page.Title`, добавьте следующую строку кода:</span><span class="sxs-lookup"><span data-stu-id="4f77e-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="4f77e-175">В сером поле редактирования слева от кода, нажмите кнопку рядом с этой новой строке для добавления *останова*.</span><span class="sxs-lookup"><span data-stu-id="4f77e-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="4f77e-176">Точка останова — маркер, который указывает отладчику, что для остановки на этом этапе работы программы, чтобы можно было видеть, что происходит.</span><span class="sxs-lookup"><span data-stu-id="4f77e-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Набор точек останова](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="4f77e-178">Удалите вызов `ServerInfo.GetHtml` метода и добавьте вызов `@myTime` переменной на его месте.</span><span class="sxs-lookup"><span data-stu-id="4f77e-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="4f77e-179">Этот вызов отображает текущее значение времени, возвращенный новую строку кода.</span><span class="sxs-lookup"><span data-stu-id="4f77e-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="4f77e-180">Нажмите клавишу F5 для запуска страницы в отладчике.</span><span class="sxs-lookup"><span data-stu-id="4f77e-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="4f77e-181">Страница останавливается на заданной точке останова.</span><span class="sxs-lookup"><span data-stu-id="4f77e-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="4f77e-182">На следующем рисунке, как страница выглядит в редакторе с точкой останова (желтым цветом).</span><span class="sxs-lookup"><span data-stu-id="4f77e-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![точки останова для отладки](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="4f77e-184">В панели инструментов отладки нажмите **шаг с заходом** (или клавишу F11) для запуска следующей строке кода.</span><span class="sxs-lookup"><span data-stu-id="4f77e-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="4f77e-185">Каждый раз при нажатии этой кнопки переходе выполнение на следующей строке кода.</span><span class="sxs-lookup"><span data-stu-id="4f77e-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Шаг с заходом в кнопку](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="4f77e-187">Проверьте значение `myTime` переменных, удерживая указатель мыши над ним или путем проверки значения, отображаемые в **локальные** и **стек вызовов** windows.</span><span class="sxs-lookup"><span data-stu-id="4f77e-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="4f77e-188">Visual Studio отображает значение переменной.</span><span class="sxs-lookup"><span data-stu-id="4f77e-188">Visual Studio display the value of the variable.</span></span>

    ![значение отображает время](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="4f77e-190">После завершения проверки переменной и пошаговое выполнение кода, нажмите клавишу F5, чтобы продолжить выполнение страницы без остановки в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="4f77e-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="4f77e-191">После завершения всех код пошагово, браузер отображает страницу.</span><span class="sxs-lookup"><span data-stu-id="4f77e-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="4f77e-192">Дополнительные сведения об отладчике и о том, как выполнить отладку кода в Visual Studio см. в разделе [Пошаговое руководство: отладка веб-страницы в Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f77e-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="4f77e-193">С помощью Razor в проектах ASP.NET MVC с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f77e-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="4f77e-194">Синтаксис Razor также широко используется в проектах ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f77e-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="4f77e-195">MVC — эффективный, основанный на шаблонах способ создания динамических веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="4f77e-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="4f77e-196">Если веб-узла ASP.NET Web Pages становится трудно поддерживать, может потребоваться следует преобразовать его в приложение ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f77e-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="4f77e-197">Пример создания приложения MVC см. в разделе [Приступая к работе с ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="4f77e-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="4f77e-198">Установка поддержки веб-страниц ASP.NET в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="4f77e-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="4f77e-199">В этом разделе показано, как установить Visual Web Developer Express 2010 и средства веб-страниц ASP.NET для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f77e-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="4f77e-200">Если у вас еще нет установщика веб-платформы, загрузите его со следующего URL:</span><span class="sxs-lookup"><span data-stu-id="4f77e-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="4f77e-201">Запустите установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="4f77e-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="4f77e-202">Нажмите кнопку **продуктов** вкладки.</span><span class="sxs-lookup"><span data-stu-id="4f77e-202">Click the **Products** tab.</span></span>

    ![Вкладка продуктов установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="4f77e-204">Поиск **ASP.NET MVC 4** (для ASP.NET Web Pages 2) и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="4f77e-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="4f77e-205">Эти продукты включают средства Visual Studio для построения веб-сайтов ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="4f77e-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Параметры установки установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="4f77e-207">Нажмите кнопку **установить** для завершения установки.</span><span class="sxs-lookup"><span data-stu-id="4f77e-207">Click **Install** to complete the installation.</span></span>
