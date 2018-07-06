---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio | Документация Майкрософт
author: tfitzmac
description: В этом приложении объясняется, как можно использовать Visual Studio 2010 или Visual Web Developer 2010 Express программирование веб-страниц ASP.NET с использованием синтаксиса Razor.
ms.author: aspnetcontent
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 46807b464499b2e60d995d37f161ca129d38f439
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834459"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="3c898-103">Программирование веб-страниц ASP.NET (Razor) с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c898-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="3c898-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3c898-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3c898-105">В этой статье объясняется, как можно использовать Visual Studio или Visual Web Developer Express программу веб-сайтов ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="3c898-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="3c898-106">Вы узнаете, как</span><span class="sxs-lookup"><span data-stu-id="3c898-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="3c898-107">Что необходимо для установки (при ее наличии) для работы с веб-страниц ASP.NET в вашей версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c898-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="3c898-108">Как добавить поддержку для веб-страниц ASP.NET в Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="3c898-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="3c898-109">Как использовать функции в Visual Studio для работы с ASP.NET Razor pages, включая IntelliSense и отладчик.</span><span class="sxs-lookup"><span data-stu-id="3c898-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3c898-110">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="3c898-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3c898-111">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3c898-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="3c898-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3c898-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="3c898-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="3c898-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="3c898-114">Этот учебник также работает с веб-страниц ASP.NET 2, Visual Studio 2012, Visual Studio 2010 и WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="3c898-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="3c898-115">Можно программировать веб-страницы ASP.NET с синтаксисом Razor с помощью WebMatrix или других редакторов кода.</span><span class="sxs-lookup"><span data-stu-id="3c898-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="3c898-116">Также можно использовать Microsoft Visual Studio, который является это полнофункциональная интегрированная среда разработки (IDE), предоставляет мощный набор средств для создания различных типов приложений (не только веб-сайтов).</span><span class="sxs-lookup"><span data-stu-id="3c898-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="3c898-117">Для работы с ASP.NET Razor pages, вы можете либо воспользуйтесь одним из полных выпусках Visual Studio или бесплатного [Visual Studio Express для Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span><span class="sxs-lookup"><span data-stu-id="3c898-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="3c898-118">Ниже приведены два особенно полезных возможностей, предоставляемых Visual Studio для программирования с использованием веб-страниц ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="3c898-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="3c898-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="3c898-119">*IntelliSense*.</span></span> <span data-ttu-id="3c898-120">Функция IntelliSense, встроенные в Visual Studio осуществляется в большем, чем IntelliSense в WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3c898-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="3c898-121">*Отладчик*.</span><span class="sxs-lookup"><span data-stu-id="3c898-121">*Debugger*.</span></span> <span data-ttu-id="3c898-122">Отладчик позволяет устранять кода, остановки программы во время его выполнения, изучении состояния переменных и пошаговое выполнение кода по одной строке.</span><span class="sxs-lookup"><span data-stu-id="3c898-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="3c898-123">Использование Visual Studio с разными версиями веб-страниц ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3c898-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="3c898-124">Visual Studio 2012 и Visual Studio 2013 включают поддержку для веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c898-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="3c898-125">(При установке Visual Studio будут установлены пакеты, которые требуются для поддержки веб-страниц ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="3c898-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="3c898-126">Visual Studio 2010 не поддерживает по умолчанию для веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c898-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="3c898-127">Чтобы использовать веб-страниц ASP.NET с помощью Visual Studio 2010, необходимо установить пакет ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3c898-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="3c898-128">Чтобы получить ASP.NET Web Pages 2, необходимо установить ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3c898-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="3c898-129">В следующей таблице перечислены поддержка для веб-страниц ASP.NET в разных версиях Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c898-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="3c898-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3c898-130">Visual Studio 2010</span></span> | <span data-ttu-id="3c898-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3c898-131">Visual Studio 2012</span></span> | <span data-ttu-id="3c898-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3c898-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3c898-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="3c898-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="3c898-134">Установка ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3c898-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="3c898-135">(Включено)</span><span class="sxs-lookup"><span data-stu-id="3c898-135">(Included)</span></span> | <span data-ttu-id="3c898-136">(Включено)</span><span class="sxs-lookup"><span data-stu-id="3c898-136">(Included)</span></span> |
| <span data-ttu-id="3c898-137">**Веб-страницы ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="3c898-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="3c898-138">Обновление ASP.NET Web Pages 3 с помощью NuGet</span><span class="sxs-lookup"><span data-stu-id="3c898-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="3c898-139">(Включено)</span><span class="sxs-lookup"><span data-stu-id="3c898-139">(Included)</span></span> |

<span data-ttu-id="3c898-140">Для работы с Visual Studio 2010, см. в разделе [Установка поддержки для веб-страниц ASP.NET в Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="3c898-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="3c898-141">Запустите среду Visual Studio из WebMatrix</span><span class="sxs-lookup"><span data-stu-id="3c898-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="3c898-142">Если вы запустили проект в WebMatrix и переключитесь в Visual Studio, WebMatrix предоставляет кнопку, чтобы легко открыть проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c898-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="3c898-143">Необходимо установить Visual Studio, установленной на компьютере для данной кнопки, включить.</span><span class="sxs-lookup"><span data-stu-id="3c898-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="3c898-144">На следующем рисунке кнопки в WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3c898-144">The following image shows the button in WebMatrix.</span></span>

![Запустите Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="3c898-146">При нажатии кнопки проект открыт в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c898-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="3c898-147">Можно переключиться назад и вперед WebMatrix и Visual Studio без проблем.</span><span class="sxs-lookup"><span data-stu-id="3c898-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="3c898-148">Вы получите уведомление, если файлы были изменены в другой среде, необходимо перезагрузить для получения последних изменений.</span><span class="sxs-lookup"><span data-stu-id="3c898-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="3c898-149">Создание сайта ASP.NET Razor в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c898-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="3c898-150">Чтобы создать на веб-сайте ASP.NET Razor в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3c898-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="3c898-151">Запустите Visual Studio или Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="3c898-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="3c898-152">В **файл** меню, щелкните **новый веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="3c898-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Создание нового веб-сайта](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="3c898-154">В **новый веб-сайт** диалоговом окне выберите используемый язык (Visual C# или Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="3c898-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="3c898-155">Выберите **веб-сайт ASP.NET (Razor)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="3c898-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![узел Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="3c898-157">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3c898-157">Click **OK**.</span></span>

<span data-ttu-id="3c898-158">Проект существует и содержит некоторые веб-страницы по умолчанию, чтобы помочь вам приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="3c898-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="3c898-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="3c898-159">Using IntelliSense</span></span>

<span data-ttu-id="3c898-160">Теперь, когда вы создали узел, вы увидите сообщение о том, как работает технология IntelliSense в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c898-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="3c898-161">Откройте в веб-сайт, вы только что создали, *Default.cshtml* страницы.</span><span class="sxs-lookup"><span data-stu-id="3c898-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="3c898-162">После `<h3>` введите теги в странице `@ServerInfo.` (включая точку).</span><span class="sxs-lookup"><span data-stu-id="3c898-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="3c898-163">Функция IntelliSense покажет доступные методы для `ServerInfo` вспомогательная функция в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="3c898-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="3c898-165">Выберите `GetHtml` метод в списке и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="3c898-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="3c898-166">IntelliSense автоматически заполняет метод.</span><span class="sxs-lookup"><span data-stu-id="3c898-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="3c898-167">(Как с помощью любого метода в C#, необходимо добавить `()` символов после метода.)</span><span class="sxs-lookup"><span data-stu-id="3c898-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
   <span data-ttu-id="3c898-168">Полный код для `GetHtml` метод выглядит как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="3c898-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="3c898-169">Нажмите клавиши Ctrl + F5, чтобы запустить эту страницу.</span><span class="sxs-lookup"><span data-stu-id="3c898-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="3c898-170">Это выглядит страницы при отображению в браузере:</span><span class="sxs-lookup"><span data-stu-id="3c898-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![страница по умолчанию в браузере](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="3c898-172">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="3c898-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="3c898-173">С помощью отладчика</span><span class="sxs-lookup"><span data-stu-id="3c898-173">Using the Debugger</span></span>

1. <span data-ttu-id="3c898-174">В верхней части *Default.cshtml* страницы после строки, которая начинается с `Page.Title`, добавьте следующую строку кода:</span><span class="sxs-lookup"><span data-stu-id="3c898-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="3c898-175">В сером поле редактора слева от кода, щелкните рядом с этой новой строке, чтобы добавить *точки останова*.</span><span class="sxs-lookup"><span data-stu-id="3c898-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="3c898-176">Точка останова — маркер, который указывает отладчику, чтобы остановить выполнение программы на этом этапе, чтобы можно было видеть, что происходит.</span><span class="sxs-lookup"><span data-stu-id="3c898-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Задание точки останова](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="3c898-178">Удалите вызов `ServerInfo.GetHtml` метод и добавьте вызов `@myTime` переменной на его месте.</span><span class="sxs-lookup"><span data-stu-id="3c898-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="3c898-179">Этот вызов отображает текущее значение времени, который возвращается новая строка кода.</span><span class="sxs-lookup"><span data-stu-id="3c898-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="3c898-180">Нажмите клавишу F5, чтобы запустить эту страницу в отладчике.</span><span class="sxs-lookup"><span data-stu-id="3c898-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="3c898-181">Страницы останавливается в заданной точке останова.</span><span class="sxs-lookup"><span data-stu-id="3c898-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="3c898-182">Ниже представлен результат страницы в редакторе с точкой останова (желтым цветом).</span><span class="sxs-lookup"><span data-stu-id="3c898-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![точки останова для отладки](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="3c898-184">На панели инструментов отладки, нажмите кнопку **шаг с заходом** (или клавишу F11) для запуска следующей строке кода.</span><span class="sxs-lookup"><span data-stu-id="3c898-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="3c898-185">Каждый раз при нажатии этой кнопки выполнения вы перейдите на следующую строку кода.</span><span class="sxs-lookup"><span data-stu-id="3c898-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Шаг с заходом в кнопку](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="3c898-187">Проверьте значение `myTime` переменных, удерживая указатель мыши над ним или путем проверки значения, отображаемые в **"Локальные"** и **стек вызовов** windows.</span><span class="sxs-lookup"><span data-stu-id="3c898-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="3c898-188">Visual Studio отображает значение переменной.</span><span class="sxs-lookup"><span data-stu-id="3c898-188">Visual Studio display the value of the variable.</span></span>

    ![значение отображает время](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="3c898-190">После завершения проверки переменной и пошаговое выполнение кода, нажмите клавишу F5, чтобы продолжить выполнение страницы без остановки в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="3c898-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="3c898-191">После завершения всех код пошагово, браузер отображает страницу.</span><span class="sxs-lookup"><span data-stu-id="3c898-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="3c898-192">Дополнительные сведения об отладчике и о том, как выполнять отладку кода в Visual Studio, см. в разделе [Пошаговое руководство: отладка веб-страниц в Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c898-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="3c898-193">С помощью Razor в проектах ASP.NET MVC с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c898-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="3c898-194">Синтаксис Razor также широко используется в проектах ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3c898-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="3c898-195">MVC — это эффективный, основанный на шаблонах способ создания динамических веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="3c898-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="3c898-196">Если веб-узла ASP.NET Web Pages становится трудно поддерживать, может потребоваться его следует преобразовать его в приложение ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3c898-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="3c898-197">Пример создания приложения MVC, см. в разделе [Приступая к работе с ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="3c898-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="3c898-198">Установка поддержки веб-страниц ASP.NET в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3c898-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="3c898-199">В этом разделе показано, как установить Visual Web Developer Express 2010 и средства веб-страниц ASP.NET для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c898-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="3c898-200">Если у вас еще нет установщика веб-платформы, загрузите его со следующего URL:</span><span class="sxs-lookup"><span data-stu-id="3c898-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="3c898-201">Запустите установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="3c898-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="3c898-202">Нажмите кнопку **продуктов** вкладки.</span><span class="sxs-lookup"><span data-stu-id="3c898-202">Click the **Products** tab.</span></span>

    ![Вкладка продуктов установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="3c898-204">Поиск **ASP.NET MVC 4** (для ASP.NET Web Pages 2) и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="3c898-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="3c898-205">Эти продукты включают средства Visual Studio для создания веб-сайтов ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="3c898-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Параметры установки установщика веб-платформы](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="3c898-207">Нажмите кнопку **установить** для завершения установки.</span><span class="sxs-lookup"><span data-stu-id="3c898-207">Click **Install** to complete the installation.</span></span>
