---
uid: single-page-application/overview/templates/hottowel-template
title: Горячий полотенец шаблона | Документы Microsoft
author: madskristensen
description: Шаблон HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a><span data-ttu-id="85fd5-103">Горячий полотенец шаблона</span><span class="sxs-lookup"><span data-stu-id="85fd5-103">Hot Towel template</span></span>
====================
<span data-ttu-id="85fd5-104">по [Мэдс Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="85fd5-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="85fd5-105">Джон Папа записана горячей шаблона MVC полотенец</span><span class="sxs-lookup"><span data-stu-id="85fd5-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="85fd5-106">Выберите версию для загрузки:</span><span class="sxs-lookup"><span data-stu-id="85fd5-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="85fd5-107">Шаблон MVC горячей полотенец для Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="85fd5-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="85fd5-108">Шаблон MVC горячей полотенец для Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85fd5-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="85fd5-109">Горячий полотенец: Так как вы не хотите перейти к SPA без одного!</span><span class="sxs-lookup"><span data-stu-id="85fd5-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="85fd5-110">Для построения SPA, но не может решить, откуда начинать?</span><span class="sxs-lookup"><span data-stu-id="85fd5-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="85fd5-111">Использовать горячий полотенец и в секундах вы получите SPA и все средства, необходимые для создания на нем!</span><span class="sxs-lookup"><span data-stu-id="85fd5-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="85fd5-112">Горячий полотенец создает значительные отправной точкой для создания одной страницы приложений (SPA) с ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="85fd5-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="85fd5-113">Без дополнительной настройки можно предусматривается модульная структура вашего кода, представления навигации, привязки данных, управления сложных данных и простой, но элегантное стилем.</span><span class="sxs-lookup"><span data-stu-id="85fd5-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="85fd5-114">Горячий полотенец предоставляет все необходимое для построения SPA, позволяя сосредоточиться на приложения, не все.</span><span class="sxs-lookup"><span data-stu-id="85fd5-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="85fd5-115">Дополнительные сведения о построении SPA из [Джон Папа видеоролики, учебные материалы и курсы Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="85fd5-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="85fd5-116">Структура приложений</span><span class="sxs-lookup"><span data-stu-id="85fd5-116">Application Structure</span></span>

<span data-ttu-id="85fd5-117">Горячий SPA полотенец предоставляет папке приложения, которая содержит JavaScript и HTML-файлов, которые определяют приложение.</span><span class="sxs-lookup"><span data-stu-id="85fd5-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="85fd5-118">В папке приложения:</span><span class="sxs-lookup"><span data-stu-id="85fd5-118">Inside the App folder:</span></span>

- <span data-ttu-id="85fd5-119">durandal</span><span class="sxs-lookup"><span data-stu-id="85fd5-119">durandal</span></span>
- <span data-ttu-id="85fd5-120">службы</span><span class="sxs-lookup"><span data-stu-id="85fd5-120">services</span></span>
- <span data-ttu-id="85fd5-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="85fd5-121">viewmodels</span></span>
- <span data-ttu-id="85fd5-122">представления</span><span class="sxs-lookup"><span data-stu-id="85fd5-122">views</span></span>

<span data-ttu-id="85fd5-123">Папка приложения содержит коллекцию модулей.</span><span class="sxs-lookup"><span data-stu-id="85fd5-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="85fd5-124">Эти модули инкапсулируют функциональность и объявлять зависимости на другие модули.</span><span class="sxs-lookup"><span data-stu-id="85fd5-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="85fd5-125">Папка views содержит HTML-код для приложения и папке viewmodels содержит логику представления для представлений (MVVM шаблоне общего).</span><span class="sxs-lookup"><span data-stu-id="85fd5-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="85fd5-126">Папка служб идеально подходит для размещения любые общие службы, в приложении могут понадобиться, например извлечение данных HTTP или для взаимодействия локального хранилища.</span><span class="sxs-lookup"><span data-stu-id="85fd5-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="85fd5-127">Часто бывает несколько viewmodels для повторного использования кода из модулей службы.</span><span class="sxs-lookup"><span data-stu-id="85fd5-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="85fd5-128">Структура приложений стороне сервера ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="85fd5-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="85fd5-129">Горячий полотенец основан на структуре знакомая и мощная ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="85fd5-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="85fd5-130">Приложение\_запуск</span><span class="sxs-lookup"><span data-stu-id="85fd5-130">App\_Start</span></span>
- <span data-ttu-id="85fd5-131">Content</span><span class="sxs-lookup"><span data-stu-id="85fd5-131">Content</span></span>
- <span data-ttu-id="85fd5-132">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="85fd5-132">Controllers</span></span>
- <span data-ttu-id="85fd5-133">Модели</span><span class="sxs-lookup"><span data-stu-id="85fd5-133">Models</span></span>
- <span data-ttu-id="85fd5-134">Сценарии</span><span class="sxs-lookup"><span data-stu-id="85fd5-134">Scripts</span></span>
- <span data-ttu-id="85fd5-135">Представления</span><span class="sxs-lookup"><span data-stu-id="85fd5-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="85fd5-136">Основные библиотеки</span><span class="sxs-lookup"><span data-stu-id="85fd5-136">Featured Libraries</span></span>

- <span data-ttu-id="85fd5-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="85fd5-137">ASP.NET MVC</span></span>
- <span data-ttu-id="85fd5-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="85fd5-138">ASP.NET Web API</span></span>
- <span data-ttu-id="85fd5-139">Оптимизация ASP.NET Web - объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="85fd5-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="85fd5-140">[Breeze.js](http://Breezejs.com) -сложных данных управления</span><span class="sxs-lookup"><span data-stu-id="85fd5-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="85fd5-141">[Durandal.js](http://Durandaljs.com) -навигации и создание представлений</span><span class="sxs-lookup"><span data-stu-id="85fd5-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="85fd5-142">[Knockout.js](http://Knockoutjs.com) -привязки данных</span><span class="sxs-lookup"><span data-stu-id="85fd5-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="85fd5-143">[Require.js](http://requirejs.org) -модульности с AMD и оптимизации</span><span class="sxs-lookup"><span data-stu-id="85fd5-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="85fd5-144">[Toastr.js](http://jpapa.me/c7toastr) -всплывающих сообщений</span><span class="sxs-lookup"><span data-stu-id="85fd5-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="85fd5-145">[Twitter начальной загрузки](http://twitter.github.com/bootstrap/) — надежный Дизайн CSS</span><span class="sxs-lookup"><span data-stu-id="85fd5-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="85fd5-146">Установка через шаблон SPA горячей полотенец Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="85fd5-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="85fd5-147">Горячий полотенец можно установить как шаблон Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="85fd5-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="85fd5-148">Просто щелкните `File`  |  `New Project` и выберите `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="85fd5-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="85fd5-149">Затем выберите "горячей полотенец одностраничного приложения «шаблон и выполнения!</span><span class="sxs-lookup"><span data-stu-id="85fd5-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="85fd5-150">Установка с помощью пакета NuGet</span><span class="sxs-lookup"><span data-stu-id="85fd5-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="85fd5-151">Горячий полотенец также является пакета NuGet, который расширяет существующий пустой проект ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="85fd5-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="85fd5-152">Просто установите с помощью Nuget, а затем запустите!</span><span class="sxs-lookup"><span data-stu-id="85fd5-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="85fd5-153">Как создавать на активном полотенец?</span><span class="sxs-lookup"><span data-stu-id="85fd5-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="85fd5-154">Просто приступите к добавлению кода!</span><span class="sxs-lookup"><span data-stu-id="85fd5-154">Simply start adding code!</span></span>

1. <span data-ttu-id="85fd5-155">Добавить собственные серверный код, предпочтительно Entity Framework и WebAPI (которых предстают Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="85fd5-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="85fd5-156">Добавление представлений для `App/views` папки</span><span class="sxs-lookup"><span data-stu-id="85fd5-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="85fd5-157">Добавить viewmodels для `App/viewmodels` папки</span><span class="sxs-lookup"><span data-stu-id="85fd5-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="85fd5-158">Добавление нового представления HTML и частичной привязки данных</span><span class="sxs-lookup"><span data-stu-id="85fd5-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="85fd5-159">Обновление маршрутов навигации в `shell.js`</span><span class="sxs-lookup"><span data-stu-id="85fd5-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="85fd5-160">Пошаговое руководство по HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="85fd5-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="85fd5-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="85fd5-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="85fd5-162">index.cshtml является начальной маршрута и представления для приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="85fd5-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="85fd5-163">Он содержит все стандартные мета-теги, ссылки css и JavaScript ссылки, необходимые для.</span><span class="sxs-lookup"><span data-stu-id="85fd5-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="85fd5-164">Текст содержит один `<div>` это, где все содержимое (представления) будет размещаться по запросу.</span><span class="sxs-lookup"><span data-stu-id="85fd5-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="85fd5-165">`@Scripts.Render` Использует Require.js должна функционировать точка входа для кода приложения, который содержится в `main.js` файла.</span><span class="sxs-lookup"><span data-stu-id="85fd5-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="85fd5-166">Экран-заставка предоставляется демонстрируют, как создать экран-заставку при загрузке приложения.</span><span class="sxs-lookup"><span data-stu-id="85fd5-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="85fd5-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="85fd5-167">App/main.js</span></span>

<span data-ttu-id="85fd5-168">`main.js` Файл содержит код, который будет выполняться сразу после загрузки приложения.</span><span class="sxs-lookup"><span data-stu-id="85fd5-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="85fd5-169">Это место для определения маршрутов вашей навигации, задать вашей запуска представления и выполнять любой установки или начальной загрузкой например priming данных приложения.</span><span class="sxs-lookup"><span data-stu-id="85fd5-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="85fd5-170">`main.js` Файла определяет несколько модулей durandal для приложения назначим запуска.</span><span class="sxs-lookup"><span data-stu-id="85fd5-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="85fd5-171">Определение инструкции помогает устранить зависимости модулей, чтобы они были доступны для функции.</span><span class="sxs-lookup"><span data-stu-id="85fd5-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="85fd5-172">Сначала сообщения отладки включены, которой отправки сообщений о какие события, что приложение работает в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="85fd5-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="85fd5-173">Код app.start сообщает durandal framework для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="85fd5-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="85fd5-174">Условные обозначения заданы, поэтому durandal знает все представления и viewmodels находятся в тех же папках именованный соответственно.</span><span class="sxs-lookup"><span data-stu-id="85fd5-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="85fd5-175">Наконец `app.setRoot` команда загружает `shell` с помощью предопределенного `entrance` анимации.</span><span class="sxs-lookup"><span data-stu-id="85fd5-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="85fd5-176">Представления</span><span class="sxs-lookup"><span data-stu-id="85fd5-176">Views</span></span>

<span data-ttu-id="85fd5-177">Представления можно найти на `App/views` папки.</span><span class="sxs-lookup"><span data-stu-id="85fd5-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="85fd5-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="85fd5-178">shell.html</span></span>

<span data-ttu-id="85fd5-179">`shell.html` Содержит образец разметки HTML.</span><span class="sxs-lookup"><span data-stu-id="85fd5-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="85fd5-180">Все другие представления будет состоять где-нибудь в части вашего `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="85fd5-181">Предоставляет горячей полотенец `shell` с тремя областями: заголовок, область содержимого и нижний колонтитул.</span><span class="sxs-lookup"><span data-stu-id="85fd5-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="85fd5-182">Каждой из этих областей загружен вместе с другими представлениями при запросе образуют содержимое.</span><span class="sxs-lookup"><span data-stu-id="85fd5-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="85fd5-183">`compose` Привязки для колонтитулов жестко закодированы в активном полотенец, чтобы она указывала на `nav` и `footer` соответственно представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="85fd5-184">Создать привязку для раздела `#content` привязан к `router` модуля активный элемент.</span><span class="sxs-lookup"><span data-stu-id="85fd5-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="85fd5-185">Другими словами при нажатии кнопки ссылки навигации, он является соответствующее представление загружается в этой области.</span><span class="sxs-lookup"><span data-stu-id="85fd5-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="85fd5-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="85fd5-186">nav.html</span></span>

<span data-ttu-id="85fd5-187">`nav.html` Ссылки навигации для SPA.</span><span class="sxs-lookup"><span data-stu-id="85fd5-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="85fd5-188">Это структура меню можно расположение, например.</span><span class="sxs-lookup"><span data-stu-id="85fd5-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="85fd5-189">Обычно это данные, привязываемые (с помощью Knockout) позволяет `router` модуль для отображения навигации, определенные в `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="85fd5-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="85fd5-190">Маскировать ищет атрибуты выполнить привязку данных и привязывает их к `shell` viewmodel отобразить маршруты навигации и отображения progressbar (с использованием Twitter начальной загрузки) Если `router` модуль находится в процессе перемещения от одного представления к другим (см. `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="85fd5-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="85fd5-191">файл Home.HTML и details.html</span><span class="sxs-lookup"><span data-stu-id="85fd5-191">home.html and details.html</span></span>

<span data-ttu-id="85fd5-192">Эти представления содержат HTML для представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="85fd5-193">Когда `home` ссылку в `nav` при выборе этого представления меню, `home` представления будут помещены в области содержимого `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="85fd5-194">Эти представления можно дополнить или заменить на пользовательские представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="85fd5-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="85fd5-195">footer.html</span></span>

<span data-ttu-id="85fd5-196">`footer.html` Содержит код HTML, который отображается в нижнем колонтитуле, в нижней части `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="85fd5-197">Модели представлений</span><span class="sxs-lookup"><span data-stu-id="85fd5-197">ViewModels</span></span>

<span data-ttu-id="85fd5-198">ViewModels находятся в `App/viewmodels` папки.</span><span class="sxs-lookup"><span data-stu-id="85fd5-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="85fd5-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="85fd5-199">shell.js</span></span>

<span data-ttu-id="85fd5-200">`shell` Viewmodel содержит свойства и методы, которые привязаны к `shell` представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="85fd5-201">Часто это, где приведены меню навигации привязки (см. `router.mapNav` логики).</span><span class="sxs-lookup"><span data-stu-id="85fd5-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="85fd5-202">файл Home.js и details.js</span><span class="sxs-lookup"><span data-stu-id="85fd5-202">home.js and details.js</span></span>

<span data-ttu-id="85fd5-203">Эти viewmodels содержат свойства и функции, которые привязаны к `home` представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="85fd5-204">Он также содержит логику для представления презентации и связь между данными и представления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="85fd5-205">Службы</span><span class="sxs-lookup"><span data-stu-id="85fd5-205">Services</span></span>

<span data-ttu-id="85fd5-206">Службы находятся в папке приложения или службы.</span><span class="sxs-lookup"><span data-stu-id="85fd5-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="85fd5-207">В идеале вашей будущих служб, таких как модуль dataservice, который отвечает за получение и отправку удаленным данным, могут быть размещены.</span><span class="sxs-lookup"><span data-stu-id="85fd5-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="85fd5-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="85fd5-208">logger.js</span></span>

<span data-ttu-id="85fd5-209">Предоставляет горячей полотенец `logger` модуль в папку «службы».</span><span class="sxs-lookup"><span data-stu-id="85fd5-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="85fd5-210">`logger` Модуль идеально подходит для ведения журнала сообщений в консоль и пользователю в всплывающем всплывающие уведомления.</span><span class="sxs-lookup"><span data-stu-id="85fd5-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
