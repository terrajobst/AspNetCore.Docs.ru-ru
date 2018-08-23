---
uid: single-page-application/overview/templates/hottowel-template
title: "\"Горячий\" шаблон выкинуть | Документация Майкрософт"
author: madskristensen
description: Шаблон HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829049"
---
<a name="hot-towel-template"></a><span data-ttu-id="464c3-103">Шаблон Hot Towel</span><span class="sxs-lookup"><span data-stu-id="464c3-103">Hot Towel template</span></span>
====================
<span data-ttu-id="464c3-104">по [Мэдсом Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="464c3-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="464c3-105">"Горячий" шаблон MVC полотенца, написанной Джон Папа</span><span class="sxs-lookup"><span data-stu-id="464c3-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="464c3-106">Выберите версию для загрузки:</span><span class="sxs-lookup"><span data-stu-id="464c3-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="464c3-107">"Горячий" выкинуть шаблона MVC для Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="464c3-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="464c3-108">"Горячий" выкинуть шаблона MVC для Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="464c3-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="464c3-109">"Горячий" выкинуть: Так как вы не хотите перейти к SPA без него!</span><span class="sxs-lookup"><span data-stu-id="464c3-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="464c3-110">Требуется для создания SPA, но не можете решить, с которого следует начать?</span><span class="sxs-lookup"><span data-stu-id="464c3-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="464c3-111">Используйте выкинуть "Горячий" и в секунд вы получите SPA, все средства, необходимые для создания на нем!</span><span class="sxs-lookup"><span data-stu-id="464c3-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="464c3-112">"Горячий" выкинуть создает это идеальное начало для создания одностраничных приложений (SPA) с помощью ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="464c3-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="464c3-113">Изначально вы обеспечивается модульная структура для кода, навигация по представлению, привязки данных, с богатыми возможностями управления и простой, но элегантного стилей.</span><span class="sxs-lookup"><span data-stu-id="464c3-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="464c3-114">"Горячий" выкинуть предоставляет все необходимое для создания SPA, позволяя вам сосредоточиться на своем приложении, а не инфраструктуру.</span><span class="sxs-lookup"><span data-stu-id="464c3-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="464c3-115">Дополнительные сведения о построении SPA из [Джон Папа видео, учебных пособий и курсов Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="464c3-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="464c3-116">Структура приложений</span><span class="sxs-lookup"><span data-stu-id="464c3-116">Application Structure</span></span>

<span data-ttu-id="464c3-117">"Горячий" SPA выкинуть предоставляет папки приложения, который содержит файлы JavaScript и HTML, которые определяют приложение.</span><span class="sxs-lookup"><span data-stu-id="464c3-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="464c3-118">Внутри папки приложения:</span><span class="sxs-lookup"><span data-stu-id="464c3-118">Inside the App folder:</span></span>

- <span data-ttu-id="464c3-119">durandal</span><span class="sxs-lookup"><span data-stu-id="464c3-119">durandal</span></span>
- <span data-ttu-id="464c3-120">службы</span><span class="sxs-lookup"><span data-stu-id="464c3-120">services</span></span>
- <span data-ttu-id="464c3-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="464c3-121">viewmodels</span></span>
- <span data-ttu-id="464c3-122">представления</span><span class="sxs-lookup"><span data-stu-id="464c3-122">views</span></span>

<span data-ttu-id="464c3-123">Папка приложения содержит коллекцию модулей.</span><span class="sxs-lookup"><span data-stu-id="464c3-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="464c3-124">Эти модули инкапсулируют функциональность и объявлять зависимости на другие модули.</span><span class="sxs-lookup"><span data-stu-id="464c3-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="464c3-125">Папка views содержит HTML-код для приложения и в папку viewmodels содержит логику представления для представлений (общий шаблон MVVM).</span><span class="sxs-lookup"><span data-stu-id="464c3-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="464c3-126">Папка служб идеально подходит для размещения любые общие службы, которые могут потребоваться приложению, такие как получение данных HTTP или взаимодействия локального хранилища.</span><span class="sxs-lookup"><span data-stu-id="464c3-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="464c3-127">Довольно часто несколько viewmodels для повторного использования кода из модулей службы.</span><span class="sxs-lookup"><span data-stu-id="464c3-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="464c3-128">Структура приложений на стороне сервера ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="464c3-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="464c3-129">"Горячий" выкинуть основана на структуре знакомая и мощная ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="464c3-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="464c3-130">Приложение\_запуск</span><span class="sxs-lookup"><span data-stu-id="464c3-130">App\_Start</span></span>
- <span data-ttu-id="464c3-131">Content</span><span class="sxs-lookup"><span data-stu-id="464c3-131">Content</span></span>
- <span data-ttu-id="464c3-132">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="464c3-132">Controllers</span></span>
- <span data-ttu-id="464c3-133">Модели</span><span class="sxs-lookup"><span data-stu-id="464c3-133">Models</span></span>
- <span data-ttu-id="464c3-134">Сценарии</span><span class="sxs-lookup"><span data-stu-id="464c3-134">Scripts</span></span>
- <span data-ttu-id="464c3-135">Представления</span><span class="sxs-lookup"><span data-stu-id="464c3-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="464c3-136">Избранные библиотеки</span><span class="sxs-lookup"><span data-stu-id="464c3-136">Featured Libraries</span></span>

- <span data-ttu-id="464c3-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="464c3-137">ASP.NET MVC</span></span>
- <span data-ttu-id="464c3-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="464c3-138">ASP.NET Web API</span></span>
- <span data-ttu-id="464c3-139">Оптимизация веб-ASP.NET - объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="464c3-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="464c3-140">[Breeze.js](http://Breezejs.com) -с богатыми возможностями управления</span><span class="sxs-lookup"><span data-stu-id="464c3-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="464c3-141">[Durandal.js](http://Durandaljs.com) -навигации и создание представлений</span><span class="sxs-lookup"><span data-stu-id="464c3-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="464c3-142">[Knockout.js](http://Knockoutjs.com) -привязки данных</span><span class="sxs-lookup"><span data-stu-id="464c3-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="464c3-143">[Require.js](http://requirejs.org) — модульность с AMD и оптимизации</span><span class="sxs-lookup"><span data-stu-id="464c3-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="464c3-144">[Toastr.js](http://jpapa.me/c7toastr) -всплывающих сообщений</span><span class="sxs-lookup"><span data-stu-id="464c3-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="464c3-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) — надежные Дизайн CSS</span><span class="sxs-lookup"><span data-stu-id="464c3-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="464c3-146">Установка с помощью шаблона Visual Studio 2012 "Горячий" выкинуть SPA</span><span class="sxs-lookup"><span data-stu-id="464c3-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="464c3-147">"Горячий" выкинуть можно установить как шаблон Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="464c3-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="464c3-148">Просто щелкните `File`  |  `New Project` и выберите `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="464c3-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="464c3-149">Затем выберите "горячей выкинуть одностраничного приложения» шаблон и выполнения!</span><span class="sxs-lookup"><span data-stu-id="464c3-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="464c3-150">Установка с помощью пакета NuGet</span><span class="sxs-lookup"><span data-stu-id="464c3-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="464c3-151">"Горячий" выкинуть тоже пакет NuGet, который расширяет существующий пустой проект ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="464c3-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="464c3-152">Просто установите с помощью Nuget, а затем запустите!</span><span class="sxs-lookup"><span data-stu-id="464c3-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="464c3-153">Как создать на "Горячий" выкинуть?</span><span class="sxs-lookup"><span data-stu-id="464c3-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="464c3-154">Просто приступите к добавлению кода!</span><span class="sxs-lookup"><span data-stu-id="464c3-154">Simply start adding code!</span></span>

1. <span data-ttu-id="464c3-155">Добавьте свой код на стороне сервера, предпочтительно Entity Framework и веб-API (которые предстают Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="464c3-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="464c3-156">Добавление представлений для `App/views` папки</span><span class="sxs-lookup"><span data-stu-id="464c3-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="464c3-157">Добавление viewmodels `App/viewmodels` папки</span><span class="sxs-lookup"><span data-stu-id="464c3-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="464c3-158">Добавить HTML и Knockout привязки данных в новые представления</span><span class="sxs-lookup"><span data-stu-id="464c3-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="464c3-159">Обновление маршрутов навигации в `shell.js`</span><span class="sxs-lookup"><span data-stu-id="464c3-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="464c3-160">Пошаговое руководство по HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="464c3-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="464c3-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="464c3-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="464c3-162">index.cshtml является начальной маршрута и представления для приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="464c3-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="464c3-163">Он содержит все стандартные теги meta, ссылки css и JavaScript ссылки, необходимые для.</span><span class="sxs-lookup"><span data-stu-id="464c3-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="464c3-164">Текст содержит один `<div>` это, где все содержимое (представлений) будут размещаться по запросу.</span><span class="sxs-lookup"><span data-stu-id="464c3-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="464c3-165">`@Scripts.Render` Использует для выполнения точки входа для кода приложения, который содержится в Require.js `main.js` файл.</span><span class="sxs-lookup"><span data-stu-id="464c3-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="464c3-166">Чтобы продемонстрировать, как создать экран-заставку во время загрузки вашего приложения предоставляется экрана-заставки.</span><span class="sxs-lookup"><span data-stu-id="464c3-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="464c3-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="464c3-167">App/main.js</span></span>

<span data-ttu-id="464c3-168">`main.js` Файл содержит код, который будет выполняться сразу после загрузки приложения.</span><span class="sxs-lookup"><span data-stu-id="464c3-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="464c3-169">Это место определять маршруты навигации, задайте запуска представления и выполнять любой программы установки и начальной загрузки такие как Подготовка данных приложения.</span><span class="sxs-lookup"><span data-stu-id="464c3-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="464c3-170">`main.js` Файл определяет несколько модулей durandal для приложения назначим запуск.</span><span class="sxs-lookup"><span data-stu-id="464c3-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="464c3-171">Определить оператор помогает устранить зависимости модулей, чтобы они стали доступны для функции.</span><span class="sxs-lookup"><span data-stu-id="464c3-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="464c3-172">Сначала сообщения отладки включены, что отправлять сообщения о том, какие события выполняет приложение в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="464c3-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="464c3-173">Код app.start указывает платформе durandal для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="464c3-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="464c3-174">Правила заданы, поэтому durandal знает все представления и viewmodels содержатся в тех же папках именованный, соответственно.</span><span class="sxs-lookup"><span data-stu-id="464c3-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="464c3-175">Наконец `app.setRoot` запускает загружает `shell` с помощью предопределенного `entrance` анимации.</span><span class="sxs-lookup"><span data-stu-id="464c3-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="464c3-176">Представления</span><span class="sxs-lookup"><span data-stu-id="464c3-176">Views</span></span>

<span data-ttu-id="464c3-177">Представления можно найти в `App/views` папку.</span><span class="sxs-lookup"><span data-stu-id="464c3-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="464c3-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="464c3-178">shell.html</span></span>

<span data-ttu-id="464c3-179">`shell.html` Содержит основной макет для HTML.</span><span class="sxs-lookup"><span data-stu-id="464c3-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="464c3-180">Все другие представления будет состоять где-нибудь в части вашего `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="464c3-181">Предоставляет горячего выкинуть `shell` с тремя областями: заголовок, область содержимого и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="464c3-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="464c3-182">Каждый из этих регионов загружается с другими представлениями при запросе образуют содержимое.</span><span class="sxs-lookup"><span data-stu-id="464c3-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="464c3-183">`compose` Привязки для заголовка и нижнего колонтитула была жестко задана в активном полотенца, чтобы он указывал `nav` и `footer` соответственно представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="464c3-184">Создать привязку для раздела `#content` привязан к `router` модуля активный элемент.</span><span class="sxs-lookup"><span data-stu-id="464c3-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="464c3-185">Другими словами Если щелкнуть ссылку навигации, это соответствующее представление загружается в этой области.</span><span class="sxs-lookup"><span data-stu-id="464c3-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="464c3-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="464c3-186">nav.html</span></span>

<span data-ttu-id="464c3-187">`nav.html` Ссылки навигации для SPA.</span><span class="sxs-lookup"><span data-stu-id="464c3-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="464c3-188">Это где структуре меню могут быть помещены, например.</span><span class="sxs-lookup"><span data-stu-id="464c3-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="464c3-189">Часто это данные, привязанные (с помощью Knockout) для `router` модуля для отображения структуры переходов, определенные в `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="464c3-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="464c3-190">Knockout ищет атрибуты привязки данных и привязывает их к `shell` модели представления для отображения маршрутов навигации и отображения progressbar (с помощью Twitter Bootstrap) Если `router` модуль находится в середине перехода от одного представления к другой (см. `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="464c3-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="464c3-191">файл Home.HTML и details.html</span><span class="sxs-lookup"><span data-stu-id="464c3-191">home.html and details.html</span></span>

<span data-ttu-id="464c3-192">Эти представления содержат HTML для представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="464c3-193">Когда `home` ссылку в `nav` при выборе меню представления, `home` представление будет помещено в область содержимого `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="464c3-194">Эти представления можно дополнить или заменить на собственные пользовательские представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="464c3-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="464c3-195">footer.html</span></span>

<span data-ttu-id="464c3-196">`footer.html` Содержит HTML, отображаемый в нижнем колонтитуле, в нижней части `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="464c3-197">Модели представлений</span><span class="sxs-lookup"><span data-stu-id="464c3-197">ViewModels</span></span>

<span data-ttu-id="464c3-198">Модели ViewModel можно найти в `App/viewmodels` папку.</span><span class="sxs-lookup"><span data-stu-id="464c3-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="464c3-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="464c3-199">shell.js</span></span>

<span data-ttu-id="464c3-200">`shell` Viewmodel содержит свойства и функции, которые привязаны к `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="464c3-201">Часто это, где приведены привязки меню навигации (см. в разделе `router.mapNav` логики).</span><span class="sxs-lookup"><span data-stu-id="464c3-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="464c3-202">файл Home.js и details.js</span><span class="sxs-lookup"><span data-stu-id="464c3-202">home.js and details.js</span></span>

<span data-ttu-id="464c3-203">Эти классы ViewModel содержат свойства и функции, которые привязаны к `home` представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="464c3-204">Он также содержит логику представления для представления и связь между данными и представления.</span><span class="sxs-lookup"><span data-stu-id="464c3-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="464c3-205">Службы</span><span class="sxs-lookup"><span data-stu-id="464c3-205">Services</span></span>

<span data-ttu-id="464c3-206">Службы можно найти в папке приложений и служб.</span><span class="sxs-lookup"><span data-stu-id="464c3-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="464c3-207">В идеале в будущих служб, таких как модуль dataservice, который отвечает за получение и отправку удаленным данным, может размещаться.</span><span class="sxs-lookup"><span data-stu-id="464c3-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="464c3-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="464c3-208">logger.js</span></span>

<span data-ttu-id="464c3-209">Предоставляет горячего выкинуть `logger` модуль в папку «службы».</span><span class="sxs-lookup"><span data-stu-id="464c3-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="464c3-210">`logger` Модуль идеально подходит для ведения журнала сообщений в консоль и пользователю в всплывающем всплывающие уведомления.</span><span class="sxs-lookup"><span data-stu-id="464c3-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
