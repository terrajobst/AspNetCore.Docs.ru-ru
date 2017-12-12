---
uid: visual-studio/overview/2013/release-notes
title: "ASP.NET и веб-инструменты Visual Studio 2013 заметки о выпуске | Документы Microsoft"
author: microsoft
description: "В этом документе описывается выпуск ASP.NET и веб-инструменты для Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 10835c39d3bca752ed3068a23fecaaab56449e41
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a><span data-ttu-id="89f73-103">ASP.NET и веб-инструменты Visual Studio 2013 заметки о выпуске</span><span class="sxs-lookup"><span data-stu-id="89f73-103">ASP.NET and Web Tools for Visual Studio 2013 Release Notes</span></span>
====================
<span data-ttu-id="89f73-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="89f73-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="89f73-105">В этом документе описывается выпуск ASP.NET и веб-инструменты для Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="89f73-105">This document describes the release of ASP.NET and Web Tools for Visual Studio 2013.</span></span>


## <a name="contents"></a><span data-ttu-id="89f73-106">Описание</span><span class="sxs-lookup"><span data-stu-id="89f73-106">Contents</span></span>

- [<span data-ttu-id="89f73-107">Замечания по установке</span><span class="sxs-lookup"><span data-stu-id="89f73-107">Installation Notes</span></span>](#TOC1)
- [<span data-ttu-id="89f73-108">Документация</span><span class="sxs-lookup"><span data-stu-id="89f73-108">Documentation</span></span>](#TOC2)
- [<span data-ttu-id="89f73-109">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="89f73-109">Software Requirements</span></span>](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="89f73-110">Новые возможности в ASP.NET и веб-инструменты для Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="89f73-110">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

- [<span data-ttu-id="89f73-111">Один ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-111">One ASP.NET</span></span>](#TOC6)
- [<span data-ttu-id="89f73-112">Новые возможности веб-проекта</span><span class="sxs-lookup"><span data-stu-id="89f73-112">New Web Project Experience</span></span>](#newproj)
- [<span data-ttu-id="89f73-113">Формирование шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-113">ASP.NET Scaffolding</span></span>](#scaffold)
- [<span data-ttu-id="89f73-114">Связь с браузером</span><span class="sxs-lookup"><span data-stu-id="89f73-114">Browser Link</span></span>](#browser-link)
- [<span data-ttu-id="89f73-115">Усовершенствования редактора Visual Studio Web</span><span class="sxs-lookup"><span data-stu-id="89f73-115">Visual Studio Web Editor Enhancements</span></span>](#web-editor)
- [<span data-ttu-id="89f73-116">Поддержка приложений веб-службы приложений Azure в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89f73-116">Azure App Service Web Apps Support in Visual Studio</span></span>](#waws)
- [<span data-ttu-id="89f73-117">Веб-публикация усовершенствования</span><span class="sxs-lookup"><span data-stu-id="89f73-117">Web Publish Enhancements</span></span>](#publish)
- [<span data-ttu-id="89f73-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="89f73-118">NuGet 2.7</span></span>](#nuget)
- [<span data-ttu-id="89f73-119">Веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-119">ASP.NET Web Forms</span></span>](#TOC9)
- [<span data-ttu-id="89f73-120">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="89f73-120">ASP.NET MVC 5</span></span>](#TOC10)
- [<span data-ttu-id="89f73-121">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="89f73-121">ASP.NET Web API 2</span></span>](#TOC11)
- [<span data-ttu-id="89f73-122">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="89f73-122">ASP.NET SignalR</span></span>](#TOC13)
- [<span data-ttu-id="89f73-123">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="89f73-123">ASP.NET Identity</span></span>](#TOC8)
- [<span data-ttu-id="89f73-124">Компоненты Microsoft OWIN</span><span class="sxs-lookup"><span data-stu-id="89f73-124">Microsoft OWIN Components</span></span>](#TOC7)
- [<span data-ttu-id="89f73-125">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="89f73-125">Entity Framework 6</span></span>](#ef6)
- [<span data-ttu-id="89f73-126">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="89f73-126">ASP.NET Razor 3</span></span>](#TOC14)
- [<span data-ttu-id="89f73-127">Приостановка приложений ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-127">ASP.NET App Suspend</span></span>](#TOC15)
- [<span data-ttu-id="89f73-128">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="89f73-128">Known Issues and Breaking Changes</span></span>](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a><span data-ttu-id="89f73-129">Замечания по установке</span><span class="sxs-lookup"><span data-stu-id="89f73-129">Installation Notes</span></span>

<span data-ttu-id="89f73-130">ASP.NET и веб-инструменты для Visual Studio 2013, объединенными в главном установщика и могут быть загружены [здесь](https://www.asp.net/downloads).</span><span class="sxs-lookup"><span data-stu-id="89f73-130">ASP.NET and Web Tools for Visual Studio 2013 are bundled in the main installer and can be downloaded [here](https://www.asp.net/downloads).</span></span>

<a id="TOC2"></a>
## <a name="documentation"></a><span data-ttu-id="89f73-131">Документация</span><span class="sxs-lookup"><span data-stu-id="89f73-131">Documentation</span></span>

<span data-ttu-id="89f73-132">Учебники и другие сведения о ASP.NET и веб-инструменты для Visual Studio 2013 доступны из [веб-сайт ASP.NET](https://www.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="89f73-132">Tutorials and other information about ASP.NET and Web Tools for Visual Studio 2013 are available from the [ASP.NET web site](https://www.asp.net/).</span></span>

<a id="TOC4"></a>
## <a name="software-requirements"></a><span data-ttu-id="89f73-133">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="89f73-133">Software Requirements</span></span>

<span data-ttu-id="89f73-134">ASP.NET и веб-инструменты требуется Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="89f73-134">ASP.NET and Web Tools requires Visual Studio 2013.</span></span>

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="89f73-135">Новые возможности в ASP.NET и веб-инструменты для Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="89f73-135">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

<span data-ttu-id="89f73-136">В следующих разделах описаны функции, которые были введены в версии.</span><span class="sxs-lookup"><span data-stu-id="89f73-136">The following sections describe the features that have been introduced in the release.</span></span>

<a id="TOC6"></a>
## <a name="one-aspnet"></a><span data-ttu-id="89f73-137">Один ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-137">One ASP.NET</span></span>

<span data-ttu-id="89f73-138">В выпуске Visual Studio 2013 перешли этапом опыт использования технологий ASP.NET, чтобы легко можно комбинировать и соответствует те, которые требуется объединить.</span><span class="sxs-lookup"><span data-stu-id="89f73-138">With the release of Visual Studio 2013, we have taken a step towards unifying the experience of using ASP.NET technologies, so that you can easily mix and match the ones you want.</span></span> <span data-ttu-id="89f73-139">Например можно запустить проект MVC с помощью и легко добавить страницы веб-форм в проект позже или формировать веб-API в проекте веб-форм.</span><span class="sxs-lookup"><span data-stu-id="89f73-139">For example, you can start a project using MVC and easily add Web Forms pages to the project later, or scaffold Web APIs in a Web Forms project.</span></span> <span data-ttu-id="89f73-140">Один ASP.NET предназначен для упрощения автоматически разработчику для выполнения действий, который вы предпочитаете в ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-140">One ASP.NET is all about making it easier for you as a developer to do the things you love in ASP.NET.</span></span> <span data-ttu-id="89f73-141">Независимо от того, Выбор технологии может иметь уверенность в создании надежных базовая архитектура One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-141">No matter what technology you choose, you can have confidence that you are building on the trusted underlying framework of One ASP.NET.</span></span>

<a id="newproj"></a>
## <a name="new-web-project-experience"></a><span data-ttu-id="89f73-142">Новые возможности веб-проекта</span><span class="sxs-lookup"><span data-stu-id="89f73-142">New Web Project Experience</span></span>

<span data-ttu-id="89f73-143">Мы обладают улучшенной опыт в создании нового веб-проектов в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="89f73-143">We have enhanced the experience of creating new web projects in Visual Studio 2013.</span></span> <span data-ttu-id="89f73-144">В **нового веб-проекта ASP.NET** диалоговом окне можно выбрать тип проекта, настройте любое сочетание технологий (веб-формы, MVC, веб-API), настройки параметров проверки подлинности и добавьте проект модульного теста.</span><span class="sxs-lookup"><span data-stu-id="89f73-144">In the **New ASP.NET Web Project** dialog you can select the project type you want, configure any combination of technologies (Web Forms, MVC, Web API), configure authentication options, and add a unit test project.</span></span>

![Новый проект ASP.NET](release-notes/_static/image1.png)

<span data-ttu-id="89f73-146">Новое диалоговое окно позволяет изменить параметры проверки подлинности по умолчанию для многих шаблонов.</span><span class="sxs-lookup"><span data-stu-id="89f73-146">The new dialog enables you to change the default authentication options for many of the templates.</span></span> <span data-ttu-id="89f73-147">Например при создании проекта веб-форм ASP.NET можно выбрать любой из следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="89f73-147">For example, when you create an ASP.NET Web Forms project you can select any of the following options:</span></span>

- <span data-ttu-id="89f73-148">Без проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="89f73-148">No Authentication</span></span>
- <span data-ttu-id="89f73-149">Индивидуальные учетные записи (членства ASP.NET или социальных поставщика вход)</span><span class="sxs-lookup"><span data-stu-id="89f73-149">Individual User Accounts (ASP.NET membership or social provider log in)</span></span>
- <span data-ttu-id="89f73-150">Учетные записи в организации (Active Directory в веб-приложение)</span><span class="sxs-lookup"><span data-stu-id="89f73-150">Organizational Accounts (Active Directory in an internet application)</span></span>
- <span data-ttu-id="89f73-151">Проверка подлинности Windows (Active Directory в приложении интрасети)</span><span class="sxs-lookup"><span data-stu-id="89f73-151">Windows Authentication (Active Directory in an intranet application)</span></span>

![Параметры проверки подлинности](release-notes/_static/image2.png)

<span data-ttu-id="89f73-153">Дополнительные сведения о новый процесс для создания веб-проектов см. в разделе [Создание веб-проектов ASP.NET в Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-153">For more information about the new process for creating web projects, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span> <span data-ttu-id="89f73-154">Дополнительные сведения о новых параметров проверки подлинности см. в разделе [ASP.NET Identity](#TOC8) далее в этом документе.</span><span class="sxs-lookup"><span data-stu-id="89f73-154">For more information about the new authentication options, see [ASP.NET Identity](#TOC8) later in this document.</span></span>

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a><span data-ttu-id="89f73-155">Формирование шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-155">ASP.NET Scaffolding</span></span>

<span data-ttu-id="89f73-156">Формирование шаблонов ASP.NET — это платформа создания кода для веб-приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-156">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="89f73-157">Он позволяет легко добавить стандартный код в проект, который взаимодействует с моделью данных.</span><span class="sxs-lookup"><span data-stu-id="89f73-157">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="89f73-158">В предыдущих версиях Visual Studio формирование шаблонов был ограничен проектов ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="89f73-158">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="89f73-159">В Visual Studio 2013 теперь можно использовать формирование шаблонов для любого проекта ASP.NET, включая веб-форм.</span><span class="sxs-lookup"><span data-stu-id="89f73-159">With Visual Studio 2013, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="89f73-160">Visual Studio 2013 не поддерживает создание страниц для проекта веб-форм, но по-прежнему можно использовать формирование шаблонов с веб-формами путем добавления зависимостей MVC в проект.</span><span class="sxs-lookup"><span data-stu-id="89f73-160">Visual Studio 2013 does not currently support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="89f73-161">Поддержка создания страниц для веб-формы будет добавлена в будущем обновлении.</span><span class="sxs-lookup"><span data-stu-id="89f73-161">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="89f73-162">При использовании формирования шаблонов мы убедитесь, что все требуемые зависимости устанавливаются в проекте.</span><span class="sxs-lookup"><span data-stu-id="89f73-162">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="89f73-163">Например если для создания проекта веб-форм ASP.NET и затем использовать формирование шаблонов для добавления контроллера Web API, необходимые пакеты NuGet и ссылки добавляются в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="89f73-163">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="89f73-164">Чтобы добавить формирование шаблонов MVC в проект веб-форм, добавьте **новый элемент формирования шаблонов** и выберите **зависимостей MVC 5** в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="89f73-164">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="89f73-165">Существует два варианта для формирования шаблонов MVC; Минимальными и полное.</span><span class="sxs-lookup"><span data-stu-id="89f73-165">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="89f73-166">При выборе минимальной только пакеты NuGet и ссылки для ASP.NET MVC добавляются в проект.</span><span class="sxs-lookup"><span data-stu-id="89f73-166">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="89f73-167">При выборе параметра «полная» добавляются минимальные зависимости, а также необходимые файлы содержимого проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="89f73-167">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="89f73-168">Поддержка асинхронных контроллеров формирование шаблонов использует новые функции асинхронного с Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="89f73-168">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="89f73-169">Дополнительные сведения и учебники см. в разделе [Обзор формирование шаблонов ASP.NET](aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-169">For more information and tutorials, see [ASP.NET Scaffolding Overview](aspnet-scaffolding-overview.md).</span></span>

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a><span data-ttu-id="89f73-170">Связь с браузером – SignalR канал между браузером и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89f73-170">Browser Link – SignalR channel between browser and Visual Studio</span></span>

<span data-ttu-id="89f73-171">Новый [связь с браузером](using-browser-link.md) позволяет подключиться к Visual Studio нескольких браузеров и их обновление всех путем нажатия кнопки на панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="89f73-171">The new [Browser Link](using-browser-link.md) feature lets you connect multiple browsers to Visual Studio and refresh them all by clicking a button in the toolbar.</span></span> <span data-ttu-id="89f73-172">Можно подключиться нескольких браузеров сайта разработки, включая мобильных эмуляторов и щелкните "Обновить" для обновления всех браузерах все одновременно.</span><span class="sxs-lookup"><span data-stu-id="89f73-172">You can connect multiple browsers to your development site, including mobile emulators, and click refresh to refresh all the browsers all at the same time.</span></span> <span data-ttu-id="89f73-173">Связь с браузером также предоставляет API, который позволяет разработчикам создавать расширения для связи с браузером.</span><span class="sxs-lookup"><span data-stu-id="89f73-173">Browser Link also exposes an API to enable developers to write Browser Link extensions.</span></span>

![](release-notes/_static/image3.png)

<span data-ttu-id="89f73-174">Обеспечивая разработчикам возможность воспользоваться преимуществами API связи с браузером, становится возможным создание очень сложных сценариев, которые пересекает границы между Visual Studio и любого браузера, который подключен.</span><span class="sxs-lookup"><span data-stu-id="89f73-174">By enabling developers to take advantage of the Browser Link API, it becomes possible to create very advanced scenarios that crosses boundaries between Visual Studio and any browser that's connected.</span></span> <span data-ttu-id="89f73-175">Web Essentials использует API для создания единство подхода Visual Studio и средств разработчика в браузере, удаленного управления мобильных эмуляторов и многое другое.</span><span class="sxs-lookup"><span data-stu-id="89f73-175">Web Essentials takes advantage of the API to create an integrated experience between Visual Studio and the browser's developer tools, remote controlling mobile emulators and a lot more.</span></span>

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a><span data-ttu-id="89f73-176">Усовершенствования редактора Visual Studio Web</span><span class="sxs-lookup"><span data-stu-id="89f73-176">Visual Studio Web Editor Enhancements</span></span>

<span data-ttu-id="89f73-177">Visual Studio 2013 включает новый редактор HTML для Razor и HTML-файлы в веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="89f73-177">Visual Studio 2013 includes a new HTML editor for Razor files and HTML files in web applications.</span></span> <span data-ttu-id="89f73-178">Новый редактор HTML предоставляет единую унифицированную схему на базе HTML5.</span><span class="sxs-lookup"><span data-stu-id="89f73-178">The new HTML editor provides a single unified schema based on HTML5.</span></span> <span data-ttu-id="89f73-179">Завершение автоматического скобок, jQuery UI и AngularJS атрибут IntelliSense, атрибут группирования IntelliSense, идентификатор и имя класса Intellisense и других усовершенствований, включая более высокую производительность, форматирование и смарт-тегов.</span><span class="sxs-lookup"><span data-stu-id="89f73-179">It has automatic brace completion, jQuery UI and AngularJS attribute IntelliSense, attribute IntelliSense Grouping, ID and class name Intellisense, and other improvements including better performance, formatting and SmartTags.</span></span>

<span data-ttu-id="89f73-180">Следующем снимке экрана показано использование атрибута начальной загрузки IntelliSense в редакторе HTML.</span><span class="sxs-lookup"><span data-stu-id="89f73-180">The following screenshot demonstrates using Bootstrap attribute IntelliSense in the HTML editor.</span></span>

![IntelliSense в редакторе HTML](release-notes/_static/image4.png)

<span data-ttu-id="89f73-182">Кроме того, Visual Studio 2013 поставляется с обоих CoffeeScript и МЕНЕЕ встроенные редакторы.</span><span class="sxs-lookup"><span data-stu-id="89f73-182">Visual Studio 2013 also comes with both CoffeeScript and LESS editors built in.</span></span> <span data-ttu-id="89f73-183">Редактор LESS поставляется со всеми функциями ожиданием из редактора CSS и имеет определенные Intellisense для переменных и mixins по меньше документов @import цепочки.</span><span class="sxs-lookup"><span data-stu-id="89f73-183">The LESS editor comes with all the cool features from the CSS editor and has specific Intellisense for variables and mixins across all the LESS documents in the @import chain.</span></span>

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a><span data-ttu-id="89f73-184">Поддержка приложений веб-службы приложений Azure в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89f73-184">Azure App Service Web Apps Support in Visual Studio</span></span>

<span data-ttu-id="89f73-185">В Visual Studio 2013 с помощью Azure SDK для .NET 2.2, можно использовать **обозревателя серверов** взаимодействовать непосредственно с удаленного веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="89f73-185">In Visual Studio 2013 with the Azure SDK for .NET 2.2, you can use **Server Explorer** to interact directly with your remote web apps.</span></span> <span data-ttu-id="89f73-186">Можно войти с учетной записью Azure, создайте новый веб-приложений, настройка приложений, просматривать журналы в режиме реального времени и многое другое.</span><span class="sxs-lookup"><span data-stu-id="89f73-186">You can sign in to your Azure account, create new web apps, configure apps, view real-time logs, and more.</span></span> <span data-ttu-id="89f73-187">Выпущена поступающих вскоре после SDK 2.2, можно будет запустить в режиме отладки удаленно в Azure.</span><span class="sxs-lookup"><span data-stu-id="89f73-187">Coming soon after SDK 2.2 is released, you'll be able to run in debug mode remotely in Azure.</span></span> <span data-ttu-id="89f73-188">Большинство новых функций для веб-приложениях службы приложений Azure также работают в Visual Studio 2012 при установке текущего выпуска пакета Azure SDK для .NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-188">Most of the new features for Azure App Service Web Apps also work in Visual Studio 2012 when you install the current release of the Azure SDK for .NET.</span></span>

<span data-ttu-id="89f73-189">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="89f73-189">For more information, see the following resources:</span></span>

- [<span data-ttu-id="89f73-190">Создать веб-приложение ASP.NET в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="89f73-190">Create an ASP.NET web app in Azure App Service</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/)
- [<span data-ttu-id="89f73-191">Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89f73-191">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a><span data-ttu-id="89f73-192">Веб-публикация усовершенствования</span><span class="sxs-lookup"><span data-stu-id="89f73-192">Web Publish Enhancements</span></span>

<span data-ttu-id="89f73-193">Visual Studio 2013 включает новые и улучшенные возможности веб-публикация.</span><span class="sxs-lookup"><span data-stu-id="89f73-193">Visual Studio 2013 includes new and enhanced Web Publish features.</span></span> <span data-ttu-id="89f73-194">Ниже приведены некоторые из них.</span><span class="sxs-lookup"><span data-stu-id="89f73-194">Here are a few of them:</span></span>

- <span data-ttu-id="89f73-195">Легко [автоматизировать шифрования файла Web.config](https://go.microsoft.com/fwlink/?LinkId=325529).</span><span class="sxs-lookup"><span data-stu-id="89f73-195">Easily [automate Web.config file encryption](https://go.microsoft.com/fwlink/?LinkId=325529).</span></span> <span data-ttu-id="89f73-196">(Эта ссылка и следующие две точки документации в библиотеке MSDN, могут быть недоступны до вечером на 10/17.)</span><span class="sxs-lookup"><span data-stu-id="89f73-196">(This link and the following two point to documentation on MSDN that might not be available until late in the day on 10/17.)</span></span>
- <span data-ttu-id="89f73-197">Легко [автоматизировать, выполнив приложение в автономный режим во время развертывания](https://go.microsoft.com/fwlink/?LinkId=325530).</span><span class="sxs-lookup"><span data-stu-id="89f73-197">Easily [automate taking an application offline during deployment](https://go.microsoft.com/fwlink/?LinkId=325530).</span></span>
- <span data-ttu-id="89f73-198">Настройка веб-развертывание для [использовать контрольной суммы файла вместо Дата последнего изменения](https://go.microsoft.com/fwlink/?LinkId=325531) для определения, какие файлы должны копироваться на сервер.</span><span class="sxs-lookup"><span data-stu-id="89f73-198">Configure Web Deploy to [use file checksum instead of last-changed date](https://go.microsoft.com/fwlink/?LinkId=325531) for determining which files should be copied to the server.</span></span>
- <span data-ttu-id="89f73-199">Быстро публиковать отдельные выбранных файлов (включая Web.config) при использовании FTP или методов публикации файловой системы, а также с веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="89f73-199">Quickly publish individual selected files (including Web.config) when you're using the FTP or file system publish methods as well as with Web Deploy.</span></span>

<span data-ttu-id="89f73-200">Дополнительные сведения о развертывании веб-ASP.NET см. в разделе [веб-сайте ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).</span><span class="sxs-lookup"><span data-stu-id="89f73-200">For more information about ASP.NET web deployment, see [the ASP.NET site](https://go.microsoft.com/fwlink/?LinkId=322027).</span></span>

<a id="nuget"></a>
## <a name="nuget-27"></a><span data-ttu-id="89f73-201">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="89f73-201">NuGet 2.7</span></span>

<span data-ttu-id="89f73-202">NuGet 2.7 включает широкий набор новых функций, которые описаны в статье [заметки о выпуске 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="89f73-202">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="89f73-203">Эта версия NuGet также избавляет от необходимости предоставить явное согласие функция восстановления пакета NuGet для загрузки пакетов.</span><span class="sxs-lookup"><span data-stu-id="89f73-203">This version of NuGet also removes the need to provide explicit consent for NuGet's package restore feature to download packages.</span></span> <span data-ttu-id="89f73-204">Запрос согласия (и соответствующие флажки в диалоговом окне параметров NuGet) теперь предоставляется путем установки NuGet.</span><span class="sxs-lookup"><span data-stu-id="89f73-204">Consent (and the associated checkbox in NuGet's preferences dialog) is now granted by installing NuGet.</span></span> <span data-ttu-id="89f73-205">Теперь пакет восстановления по умолчанию просто работает.</span><span class="sxs-lookup"><span data-stu-id="89f73-205">Now package restore simply works by default.</span></span>

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="89f73-206">Веб-формы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-206">ASP.NET Web Forms</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="89f73-207">Один ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-207">One ASP.NET</span></span>

<span data-ttu-id="89f73-208">Шаблоны проектов веб-форм тесно интегрированы с новым интерфейсом One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-208">The Web Forms project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="89f73-209">Можно добавить MVC и веб-API поддержку в проект веб-форм, и можно настроить проверку подлинности с помощью мастера создания проекта ASP.NET один.</span><span class="sxs-lookup"><span data-stu-id="89f73-209">You can add MVC and Web API support to your Web Forms project, and you can configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="89f73-210">Дополнительные сведения см. в разделе [Создание веб-проектов ASP.NET в Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-210">For more information, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="89f73-211">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="89f73-211">ASP.NET Identity</span></span>

<span data-ttu-id="89f73-212">Шаблоны проектов веб-формы поддерживают новую платформу ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="89f73-212">The Web Forms project templates support the new ASP.NET Identity framework.</span></span> <span data-ttu-id="89f73-213">Кроме того шаблоны теперь поддерживают создание проектов веб-форм интрасети.</span><span class="sxs-lookup"><span data-stu-id="89f73-213">In addition, the templates now support creation of a Web Forms intranet project.</span></span> <span data-ttu-id="89f73-214">Дополнительные сведения см. в разделе [методы проверки подлинности](creating-web-projects-in-visual-studio.md#auth) в **Создание веб-проектов ASP.NET в Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="89f73-214">For more information, see [Authentication Methods](creating-web-projects-in-visual-studio.md#auth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="bootstrap"></a><span data-ttu-id="89f73-215">начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="89f73-215">Bootstrap</span></span>

<span data-ttu-id="89f73-216">Шаблоны веб-форм используют [начальной загрузки](http://twitter.github.io/bootstrap/) для предоставления компактный и гибкий внешний вид и поведение, можно легко настроить.</span><span class="sxs-lookup"><span data-stu-id="89f73-216">The Web Forms templates use [Bootstrap](http://twitter.github.io/bootstrap/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="89f73-217">Дополнительные сведения см. в разделе [начальной загрузки в веб-шаблоны проектов Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).</span><span class="sxs-lookup"><span data-stu-id="89f73-217">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a><span data-ttu-id="89f73-218">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="89f73-218">ASP.NET MVC 5</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="89f73-219">Один ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-219">One ASP.NET</span></span>

<span data-ttu-id="89f73-220">Шаблоны проектов Web MVC легко интегрировать в новый интерфейс One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-220">The Web MVC project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="89f73-221">Можно настроить проект MVC и настроить проверку подлинности с помощью мастера создания проектов One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-221">You can customize your MVC project and configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="89f73-222">Вводное руководство в ASP.NET MVC 5 можно найти в [Приступая к работе с ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-222">An introductory tutorial to ASP.NET MVC 5 can be found at [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="89f73-223">Сведения об обновлении проектов MVC 4 до MVC 5 см. в разделе [обновление ASP.NET MVC 4 и API веб-проекта ASP.NET MVC 5 и веб-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-223">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="89f73-224">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="89f73-224">ASP.NET Identity</span></span>

<span data-ttu-id="89f73-225">Шаблоны проектов MVC были обновлены для использования ASP.NET Identity для проверки подлинности и управления удостоверениями.</span><span class="sxs-lookup"><span data-stu-id="89f73-225">The MVC project templates have been updated to use ASP.NET Identity for authentication and identity management.</span></span> <span data-ttu-id="89f73-226">Учебник, проверки подлинности Facebook и Google и нового членства API можно найти в [создать приложение ASP.NET MVC 5 с Facebook и Google OAuth2 и входа OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) и [Создание приложения ASP.NET MVC с помощью проверки подлинности и База данных SQL и развертывания в службе приложений Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="89f73-226">A tutorial featuring Facebook and Google authentication and the new membership API can be found at [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) and [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span></span>

### <a name="bootstrap"></a><span data-ttu-id="89f73-227">начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="89f73-227">Bootstrap</span></span>

<span data-ttu-id="89f73-228">Шаблон проекта MVC был обновлен для использования [начальной загрузки](http://getbootstrap.com/) для предоставления компактный и гибкий внешний вид и поведение, можно легко настроить.</span><span class="sxs-lookup"><span data-stu-id="89f73-228">The MVC project template has been updated to use [Bootstrap](http://getbootstrap.com/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="89f73-229">Дополнительные сведения см. в разделе [начальной загрузки в веб-шаблоны проектов Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).</span><span class="sxs-lookup"><span data-stu-id="89f73-229">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="89f73-230">Фильтры проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="89f73-230">Authentication filters</span></span>

<span data-ttu-id="89f73-231">Фильтры проверки подлинности — это новый тип фильтра в ASP.NET MVC, предшествует фильтры авторизации в конвейере ASP.NET MVC и позволяют задавать проверки подлинности логику за действие, на контроллере или глобально для всех контроллеров.</span><span class="sxs-lookup"><span data-stu-id="89f73-231">Authentication filters are a new kind of filter in ASP.NET MVC that run prior to authorization filters in the ASP.NET MVC pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="89f73-232">Фильтры проверки подлинности обрабатывают учетные данные в запросе и укажите соответствующий участник.</span><span class="sxs-lookup"><span data-stu-id="89f73-232">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="89f73-233">Фильтры проверки подлинности можно также добавить запросов проверки подлинности в ответ на неавторизованных запросов.</span><span class="sxs-lookup"><span data-stu-id="89f73-233">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="89f73-234">Переопределяет фильтр</span><span class="sxs-lookup"><span data-stu-id="89f73-234">Filter overrides</span></span>

<span data-ttu-id="89f73-235">Теперь можно переопределить, какие фильтры применяются к методу указанного действия или контроллер, указав переопределение фильтра.</span><span class="sxs-lookup"><span data-stu-id="89f73-235">You can now override which filters apply to a given action method or controller by specifying an override filter.</span></span> <span data-ttu-id="89f73-236">Переопределение фильтры задать фильтр типов, которые не должны выполняться для данной области (действий или контроллера).</span><span class="sxs-lookup"><span data-stu-id="89f73-236">Override filters specify a set of filter types that should not be run for a given scope (action or controller).</span></span> <span data-ttu-id="89f73-237">Это позволяет настроить фильтры, которые применяются глобально, но затем исключить определенные глобальные фильтры от применения определенных действий или контроллеров.</span><span class="sxs-lookup"><span data-stu-id="89f73-237">This allows you to configure filters that apply globally but then exclude certain global filters from applying to specific actions or controllers.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="89f73-238">Атрибут маршрутизации</span><span class="sxs-lookup"><span data-stu-id="89f73-238">Attribute routing</span></span>

<span data-ttu-id="89f73-239">ASP.NET MVC теперь поддерживает маршрутизацией атрибутов благодаря вклад по McCall Тим, автор [http://attributerouting.net](http://attributerouting.net).</span><span class="sxs-lookup"><span data-stu-id="89f73-239">ASP.NET MVC now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="89f73-240">С маршрутизацией атрибутов можно указать свои маршруты, сопроводив действия и контроллеры.</span><span class="sxs-lookup"><span data-stu-id="89f73-240">With attribute routing you can specify your routes by annotating your actions and controllers.</span></span>

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a><span data-ttu-id="89f73-241">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="89f73-241">ASP.NET Web API 2</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="89f73-242">Атрибут маршрутизации</span><span class="sxs-lookup"><span data-stu-id="89f73-242">Attribute routing</span></span>

<span data-ttu-id="89f73-243">Теперь веб-API ASP.NET поддерживает маршрутизацией атрибутов благодаря вклад по McCall Тим, автор [http://attributerouting.net](http://attributerouting.net).</span><span class="sxs-lookup"><span data-stu-id="89f73-243">ASP.NET Web API now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="89f73-244">С маршрутизацией атрибутов можно указать свои веб-API маршруты, Аннотация действия и контроллеры следующим образом:</span><span class="sxs-lookup"><span data-stu-id="89f73-244">With attribute routing you can specify your Web API routes by annotating your actions and controllers like this:</span></span>

[!code-csharp[Main](release-notes/samples/sample1.cs)]

<span data-ttu-id="89f73-245">Атрибут маршрутизации предоставляет больший контроль над URL-адреса в веб-API.</span><span class="sxs-lookup"><span data-stu-id="89f73-245">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="89f73-246">Например можно легко задавать один контроллер API с помощью иерархии ресурсов:</span><span class="sxs-lookup"><span data-stu-id="89f73-246">For example, you can easily define a resource hierarchy using a single API controller:</span></span>

[!code-csharp[Main](release-notes/samples/sample2.cs)]

<span data-ttu-id="89f73-247">Атрибут маршрутизации также предоставляет удобный синтаксис для указания дополнительных параметров, значения по умолчанию и ограничения маршрута:</span><span class="sxs-lookup"><span data-stu-id="89f73-247">Attribute routing also provides a convenient syntax for specifying optional parameters, default values, and route constraints:</span></span>

[!code-csharp[Main](release-notes/samples/sample3.cs)]

<span data-ttu-id="89f73-248">Дополнительные сведения о маршрутизации атрибута см. в разделе [атрибут маршрутизации в веб-API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-248">For more information about attribute routing, see [Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span></span>

### <a name="oauth-20"></a><span data-ttu-id="89f73-249">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="89f73-249">OAuth 2.0</span></span>

<span data-ttu-id="89f73-250">Шаблоны проектов веб-API и одностраничного приложения теперь поддерживают авторизации с помощью OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-250">The Web API and Single Page Application project templates now support authorization using OAuth 2.0.</span></span> <span data-ttu-id="89f73-251">OAuth 2.0 — это платформа для авторизации клиента доступа к защищенным ресурсам.</span><span class="sxs-lookup"><span data-stu-id="89f73-251">OAuth 2.0 is a framework for authorizing client access to protected resources.</span></span> <span data-ttu-id="89f73-252">Он работает для различных клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="89f73-252">It works for a variety of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="89f73-253">Поддержка OAuth 2.0 основана на новый безопасности по промежуточного слоя, предоставляемые компоненты Microsoft OWIN для проверки подлинности носителя и реализация роль сервера авторизации.</span><span class="sxs-lookup"><span data-stu-id="89f73-253">Support for OAuth 2.0 is based on new security middleware provided by the Microsoft OWIN Components for bearer authentication and implementing the authorization server role.</span></span> <span data-ttu-id="89f73-254">Кроме того клиенты можно авторизовать с помощью сервера авторизации организации, например Azure Active Directory или служб федерации Active Directory в Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="89f73-254">Alternatively, clients can be authorized using an organizational authorization server, such as Azure Active Directory or ADFS in Windows Server 2012 R2.</span></span>

### <a name="odata-improvements"></a><span data-ttu-id="89f73-255">Усовершенствования OData</span><span class="sxs-lookup"><span data-stu-id="89f73-255">OData Improvements</span></span>

<span data-ttu-id="89f73-256">**Поддержка $select $разверните $batch и $value**</span><span class="sxs-lookup"><span data-stu-id="89f73-256">**Support for $select, $expand, $batch, and $value**</span></span>

<span data-ttu-id="89f73-257">ASP.NET Web API OData теперь имеет полную поддержку для $select, разверните $ и $value.</span><span class="sxs-lookup"><span data-stu-id="89f73-257">ASP.NET Web API OData now has full support for $select, $expand, and $value.</span></span> <span data-ttu-id="89f73-258">$Batch также можно использовать для запроса, пакетная обработка и обработка наборов изменений.</span><span class="sxs-lookup"><span data-stu-id="89f73-258">You can also use $batch for request batching and processing of change sets.</span></span>

<span data-ttu-id="89f73-259">$Select и $expand разверните параметры позволяют изменять форму данных, возвращенный конечной точки OData.</span><span class="sxs-lookup"><span data-stu-id="89f73-259">The $select and $expand options let you change the shape of the data that is returned from an OData endpoint.</span></span> <span data-ttu-id="89f73-260">Дополнительные сведения см. в разделе [введение $select и $expand расширения набора поддерживаемых в OData веб-API](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-260">For more information, see [Introducing $select and $expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span></span>

<span data-ttu-id="89f73-261">**Улучшенная расширяемости**</span><span class="sxs-lookup"><span data-stu-id="89f73-261">**Improved extensibility**</span></span>

<span data-ttu-id="89f73-262">Модули форматирования OData теперь являются расширяемыми.</span><span class="sxs-lookup"><span data-stu-id="89f73-262">The OData formatters are now extensible.</span></span> <span data-ttu-id="89f73-263">Можно добавить запись метаданных Atom, поддерживают именованные записи связи потока и мультимедиа, добавление заметок экземпляра и настройки, как создаются ссылки.</span><span class="sxs-lookup"><span data-stu-id="89f73-263">You can add Atom entry metadata, support named stream and media link entries, add instance annotations, and customize how links are generated.</span></span>

<span data-ttu-id="89f73-264">**Поддержка типа меньше**</span><span class="sxs-lookup"><span data-stu-id="89f73-264">**Type-less support**</span></span>

<span data-ttu-id="89f73-265">Теперь можно построить служб OData без необходимости определять типы среды CLR для типов сущности.</span><span class="sxs-lookup"><span data-stu-id="89f73-265">You can now build OData services without needing to define CLR types for your entity types.</span></span> <span data-ttu-id="89f73-266">Вместо этого можно принимают или возвращают экземпляры контроллеров OData **IEdmObject**, которые являются модули форматирования OData сериализации и десериализации.</span><span class="sxs-lookup"><span data-stu-id="89f73-266">Instead, your OData controllers can take or return instances of **IEdmObject**, which are the OData formatters serialize/deserialize.</span></span>

<span data-ttu-id="89f73-267">**Повторное использование существующей модели**</span><span class="sxs-lookup"><span data-stu-id="89f73-267">**Reuse an existing model**</span></span>

<span data-ttu-id="89f73-268">Если вы уже существующей модели EDM (модель EDM), ее можно теперь повторно напрямую, вместо того, чтобы создавать новую.</span><span class="sxs-lookup"><span data-stu-id="89f73-268">If you already have an existing entity data model (EDM), you can now reuse it directly, instead of having to build a new one.</span></span> <span data-ttu-id="89f73-269">Например при использовании Entity Framework, можно использовать модель EDM, EF создает для вас.</span><span class="sxs-lookup"><span data-stu-id="89f73-269">For example, if you are using Entity Framework, you can use the EDM that EF builds for you.</span></span>

### <a name="request-batching"></a><span data-ttu-id="89f73-270">Запрос пакетной обработки</span><span class="sxs-lookup"><span data-stu-id="89f73-270">Request Batching</span></span>

<span data-ttu-id="89f73-271">Пакетная обработка запроса объединяет несколько операций в одном запросе HTTP POST, сократить сетевой трафик и обеспечить более гладким, менее «многословных» пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="89f73-271">Request batching combines multiple operations into a single HTTP POST request, to reduce network traffic and provide a smoother, less chatty user interface.</span></span> <span data-ttu-id="89f73-272">Теперь веб-API ASP.NET поддерживает несколько стратегий для пакетной обработки запроса:</span><span class="sxs-lookup"><span data-stu-id="89f73-272">ASP.NET Web API now supports several strategies for request batching:</span></span>

- <span data-ttu-id="89f73-273">Используйте $batch конечную точку службы OData.</span><span class="sxs-lookup"><span data-stu-id="89f73-273">Use the $batch endpoint of an OData service.</span></span>
- <span data-ttu-id="89f73-274">Упакуйте несколько запросов в один запрос составного MIME.</span><span class="sxs-lookup"><span data-stu-id="89f73-274">Package multiple requests into a single MIME multipart request.</span></span>
- <span data-ttu-id="89f73-275">Использование настраиваемого формата пакетной обработки.</span><span class="sxs-lookup"><span data-stu-id="89f73-275">Use a custom batching format.</span></span>

<span data-ttu-id="89f73-276">Чтобы включить запрос пакетной обработки, просто добавьте маршрут с пакетной обработки обработчика в конфигурацию веб-API:</span><span class="sxs-lookup"><span data-stu-id="89f73-276">To enable request batching, simply add a route with a batching handler to your Web API configuration:</span></span>

[!code-csharp[Main](release-notes/samples/sample4.cs)]

<span data-ttu-id="89f73-277">Можно также управлять ли запросы или если выполняется последовательно или в любом порядке.</span><span class="sxs-lookup"><span data-stu-id="89f73-277">You can also control whether requests or executed sequentially or in any order.</span></span>

### <a name="portable-aspnet-web-api-client"></a><span data-ttu-id="89f73-278">Переносимой ASP.NET Web API клиента</span><span class="sxs-lookup"><span data-stu-id="89f73-278">Portable ASP.NET Web API Client</span></span>

<span data-ttu-id="89f73-279">Теперь веб-API ASP.NET клиента можно использовать для создания переносимых библиотек классов, работающих в приложениях для магазина Windows и Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="89f73-279">You can now use the ASP.NET Web API Client to create portable class libraries that work across your Windows Store and Windows Phone 8 applications.</span></span> <span data-ttu-id="89f73-280">Можно также создать переносимый модули форматирования, которые могут совместно использоваться клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="89f73-280">You can also create portable formatters that can be shared across client and server.</span></span>

### <a name="improved-testability"></a><span data-ttu-id="89f73-281">Улучшенные возможности тестирования</span><span class="sxs-lookup"><span data-stu-id="89f73-281">Improved Testability</span></span>

<span data-ttu-id="89f73-282">Веб-API 2 делает его гораздо проще единицы к API контроллеры тестирования.</span><span class="sxs-lookup"><span data-stu-id="89f73-282">Web API 2 makes it much easier to unit test your API controllers.</span></span> <span data-ttu-id="89f73-283">Просто создать контроллер API с сообщения запроса и конфигурацией, а затем вызвать метод действия, который вы хотите проверить.</span><span class="sxs-lookup"><span data-stu-id="89f73-283">Just instantiate your API controller with your request message and configuration, and then call the action method you wish to test.</span></span> <span data-ttu-id="89f73-284">Можно также легко макета **UrlHelper** класс методов действий, которые выполняют компоновки.</span><span class="sxs-lookup"><span data-stu-id="89f73-284">It is also easy to mock the **UrlHelper** class, for action methods that perform link generation.</span></span>

### <a name="ihttpactionresult"></a><span data-ttu-id="89f73-285">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="89f73-285">IHttpActionResult</span></span>

<span data-ttu-id="89f73-286">Теперь вы можете реализовать IHttpActionResult для инкапсуляции результат действия методов веб-API.</span><span class="sxs-lookup"><span data-stu-id="89f73-286">You can now implement IHttpActionResult to encapsulate the result of your Web API action methods.</span></span> <span data-ttu-id="89f73-287">IHttpActionResult, возвращаемое из метода веб-API действие выполняется средой выполнения веб-API ASP.NET для создания результирующего ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="89f73-287">An IHttpActionResult returned from a Web API action method is executed by the ASP.NET Web API runtime to produce the resultant response message.</span></span> <span data-ttu-id="89f73-288">IHttpActionResult могут быть возвращены из любого действия веб-API для упрощения модульных тестов для реализации веб-API.</span><span class="sxs-lookup"><span data-stu-id="89f73-288">An IHttpActionResult can be returned from any Web API action to simplify unit testing of your Web API implementation.</span></span> <span data-ttu-id="89f73-289">Для удобства, предоставленные в ряде реализаций IHttpActionResult без дополнительной настройки, включая результаты для возвращения определенные коды состояния в формате содержимого или согласование содержимого ответов.</span><span class="sxs-lookup"><span data-stu-id="89f73-289">For convenience a number of IHttpActionResult implementations are provided out of the box including results for returning specific status codes, formatted content or content-negotiated responses.</span></span>

### <a name="httprequestcontext"></a><span data-ttu-id="89f73-290">HttpRequestContext</span><span class="sxs-lookup"><span data-stu-id="89f73-290">HttpRequestContext</span></span>

<span data-ttu-id="89f73-291">Новый **HttpRequestContext** отслеживает любое состояние, привязанного к запросу, но не сразу становится доступен из запроса.</span><span class="sxs-lookup"><span data-stu-id="89f73-291">The new **HttpRequestContext** tracks any state that is tied to the request but is not immediately available from the request.</span></span> <span data-ttu-id="89f73-292">Например, можно использовать **HttpRequestContext** для получения данных о маршруте, субъект, связанный с запросом, сертификат клиента, **UrlHelper** и корень виртуального пути.</span><span class="sxs-lookup"><span data-stu-id="89f73-292">For example, you can use the **HttpRequestContext** to get route data, the principal associated with the request, the client certificate, the **UrlHelper** and the virtual path root.</span></span> <span data-ttu-id="89f73-293">Можно легко создавать **HttpRequestContext** модульного тестирования.</span><span class="sxs-lookup"><span data-stu-id="89f73-293">You can easily create an **HttpRequestContext** for unit testing purposes.</span></span>

<span data-ttu-id="89f73-294">Так как участника для запроса передается вместе с запросом, не полагаясь на **Thread.CurrentPrincipal**, основного сервера теперь доступна в течение времени существования запроса находящимся в конвейер веб-API.</span><span class="sxs-lookup"><span data-stu-id="89f73-294">Because the principal for the request is flowed with the request instead of relying on **Thread.CurrentPrincipal**, the principal is now available throughout the lifetime of the request while it is in the Web API pipeline.</span></span>

### <a name="cors"></a><span data-ttu-id="89f73-295">CORS</span><span class="sxs-lookup"><span data-stu-id="89f73-295">CORS</span></span>

<span data-ttu-id="89f73-296">Благодаря другой отличных публикаций из Brock Аллен ASP.NET теперь полностью поддерживает кросс-источника запроса для управления доступом (CORS).</span><span class="sxs-lookup"><span data-stu-id="89f73-296">Thanks to another great contribution from Brock Allen, ASP.NET now fully supports Cross Origin Request Sharing (CORS).</span></span>

<span data-ttu-id="89f73-297">Безопасность обозревателя предотвращает внесение запросы AJAX в другой домен на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="89f73-297">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="89f73-298">[CORS](http://www.w3.org/TR/cors/) — это стандарт консорциума W3C, позволяет серверу ослабить политика одного источника.</span><span class="sxs-lookup"><span data-stu-id="89f73-298">[CORS](http://www.w3.org/TR/cors/) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="89f73-299">С помощью CORS, сервер можно явно разрешить некоторые запросы независимо от источника при отклонении другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="89f73-299">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span>

<span data-ttu-id="89f73-300">Веб-API 2 теперь поддерживает CORS, включая автоматическую обработку предварительные запросы.</span><span class="sxs-lookup"><span data-stu-id="89f73-300">Web API 2 now supports CORS, including automatic handling of preflight requests.</span></span> <span data-ttu-id="89f73-301">Дополнительные сведения см. в разделе [Включение запросы независимо от источника в ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-301">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="89f73-302">Фильтры проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="89f73-302">Authentication Filters</span></span>

<span data-ttu-id="89f73-303">Фильтры проверки подлинности — это новый тип фильтра в веб-API ASP.NET, запустите перед фильтры авторизации в конвейере ASP.NET Web API и позволяют задавать проверки подлинности логику за действие, на контроллере или глобально для всех контроллеров.</span><span class="sxs-lookup"><span data-stu-id="89f73-303">Authentication filters are a new kind of filter in ASP.NET Web API that run prior to authorization filters in the ASP.NET Web API pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="89f73-304">Фильтры проверки подлинности обрабатывают учетные данные в запросе и укажите соответствующий участник.</span><span class="sxs-lookup"><span data-stu-id="89f73-304">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="89f73-305">Фильтры проверки подлинности можно также добавить запросов проверки подлинности в ответ на неавторизованных запросов.</span><span class="sxs-lookup"><span data-stu-id="89f73-305">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="89f73-306">Переопределения фильтра</span><span class="sxs-lookup"><span data-stu-id="89f73-306">Filter Overrides</span></span>

<span data-ttu-id="89f73-307">Теперь можно переопределить, какие фильтры применяются к метод указанного действия или контроллер, указав переопределение фильтра.</span><span class="sxs-lookup"><span data-stu-id="89f73-307">You can now override which filters apply to a given action method or controller, by specifying an override filter.</span></span> <span data-ttu-id="89f73-308">Переопределение фильтры задать типы фильтров, которые не должны запускаться для данной области (действий или контроллера).</span><span class="sxs-lookup"><span data-stu-id="89f73-308">Override filters specify a set of filter types that should not run for a given scope (action or controller).</span></span> <span data-ttu-id="89f73-309">Это позволяет добавить глобальные фильтры, но затем исключить некоторые из конкретных действий или контроллеров.</span><span class="sxs-lookup"><span data-stu-id="89f73-309">This allows you to add global filters, but then exclude some from specific actions or controllers.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="89f73-310">Интеграция OWIN</span><span class="sxs-lookup"><span data-stu-id="89f73-310">OWIN Integration</span></span>

<span data-ttu-id="89f73-311">Веб-API ASP.NET теперь полностью поддерживает OWIN и может выполняться на любом узле поддержкой OWIN.</span><span class="sxs-lookup"><span data-stu-id="89f73-311">ASP.NET Web API now fully supports OWIN and can be run on any OWIN capable host.</span></span> <span data-ttu-id="89f73-312">Кроме того, включен **HostAuthenticationFilter** , обеспечивает интеграцию с системой проверки подлинности OWIN.</span><span class="sxs-lookup"><span data-stu-id="89f73-312">Also included is a **HostAuthenticationFilter** that provides integration with the OWIN authentication system.</span></span>

<span data-ttu-id="89f73-313">Благодаря интеграции OWIN можно резидентного размещения веб-API в процессе наряду с другими OWIN по промежуточного слоя, например SignalR.</span><span class="sxs-lookup"><span data-stu-id="89f73-313">With OWIN integration, you can self-host Web API in your own process alongside other OWIN middleware, such as SignalR.</span></span> <span data-ttu-id="89f73-314">Дополнительные сведения см. в разделе [OWIN использование веб-API ASP.NET Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-314">For more information, see [Use OWIN to Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a><span data-ttu-id="89f73-315">ASP.NET SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="89f73-315">ASP.NET SignalR 2.0</span></span>

<span data-ttu-id="89f73-316">В следующих разделах описаны возможности SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-316">The following sections describe features of SignalR 2.0.</span></span>

- [<span data-ttu-id="89f73-317">Построенные на OWIN</span><span class="sxs-lookup"><span data-stu-id="89f73-317">Built on OWIN</span></span>](#builtonowin)
- [<span data-ttu-id="89f73-318">MapHubs и MapConnection стали MapSignalR</span><span class="sxs-lookup"><span data-stu-id="89f73-318">MapHubs and MapConnection are now MapSignalR</span></span>](#MapSignalR)
- [<span data-ttu-id="89f73-319">Поддержка между доменами</span><span class="sxs-lookup"><span data-stu-id="89f73-319">Cross-Domain Support</span></span>](#crossdomain)
- [<span data-ttu-id="89f73-320">iOS и Android, поддерживают через MonoTouch и MonoDroid</span><span class="sxs-lookup"><span data-stu-id="89f73-320">iOS and Android support via MonoTouch and MonoDroid</span></span>](#mobile)
- [<span data-ttu-id="89f73-321">Клиент переносимой .NET</span><span class="sxs-lookup"><span data-stu-id="89f73-321">Portable .NET Client</span></span>](#portable)
- [<span data-ttu-id="89f73-322">Новый пакет резидентной</span><span class="sxs-lookup"><span data-stu-id="89f73-322">New Self-Host Package</span></span>](#selfhost)
- [<span data-ttu-id="89f73-323">Поддержка сервера с обратной совместимостью</span><span class="sxs-lookup"><span data-stu-id="89f73-323">Backward-compatible server support</span></span>](#backwardcompat)
- [<span data-ttu-id="89f73-324">Удалена поддержка сервера .NET 4.0</span><span class="sxs-lookup"><span data-stu-id="89f73-324">Removed server support for .NET 4.0</span></span>](#remove40)
- [<span data-ttu-id="89f73-325">Отправка сообщения в список клиентов и групп</span><span class="sxs-lookup"><span data-stu-id="89f73-325">Sending a message to a list of clients and groups</span></span>](#messagelist)
- [<span data-ttu-id="89f73-326">Отправка сообщения для определенного пользователя</span><span class="sxs-lookup"><span data-stu-id="89f73-326">Sending a message to a specific user</span></span>](#sendtouser)
- [<span data-ttu-id="89f73-327">Улучшенная поддержка обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="89f73-327">Better error handling support</span></span>](#errorhandling)
- [<span data-ttu-id="89f73-328">Проще модульного тестирования концентраторов</span><span class="sxs-lookup"><span data-stu-id="89f73-328">Easier unit testing of hubs</span></span>](#unittesting)
- [<span data-ttu-id="89f73-329">Обработка ошибок JavaScript</span><span class="sxs-lookup"><span data-stu-id="89f73-329">JavaScript error handling</span></span>](#javascripterror)

<span data-ttu-id="89f73-330">Пример обновление существующего проекта 1.x до SignalR 2.0 разделе [обновление SignalR 1.x проекта](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-330">For an example of how to upgrade an existing 1.x project to SignalR 2.0, see [Upgrading a SignalR 1.x Project](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<a id="builtonowin"></a>
### <a name="built-on-owin"></a><span data-ttu-id="89f73-331">Построенные на OWIN</span><span class="sxs-lookup"><span data-stu-id="89f73-331">Built on OWIN</span></span>

<span data-ttu-id="89f73-332">SignalR 2.0 строится на полностью [OWIN (открыть веб-интерфейс .NET)](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="89f73-332">SignalR 2.0 is built completely on [OWIN (the Open Web Interface for .NET)](http://owin.org/).</span></span> <span data-ttu-id="89f73-333">Это изменение упрощает процесс установки для SignalR, гораздо более согласованы между веб сервере и резидентных приложений SignalR, но требовал ряд изменений в API.</span><span class="sxs-lookup"><span data-stu-id="89f73-333">This change makes the setup process for SignalR much more consistent between web-hosted and self-hosted SignalR applications, but has also required a number of API changes.</span></span>

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a><span data-ttu-id="89f73-334">MapHubs и MapConnection стали MapSignalR</span><span class="sxs-lookup"><span data-stu-id="89f73-334">MapHubs and MapConnection are now MapSignalR</span></span>

<span data-ttu-id="89f73-335">Для обеспечения совместимости со стандартами OWIN эти методы были переименованы в `MapSignalR`.</span><span class="sxs-lookup"><span data-stu-id="89f73-335">For compatibility with OWIN standards, these methods have been renamed to `MapSignalR`.</span></span> <span data-ttu-id="89f73-336">`MapSignalR`вызывается без параметров будут сопоставлены всех концентраторов (как `MapHubs` и в версии 1.x); для сопоставления отдельных **подключение PersistentConnection** объектов, укажите тип соединения в качестве параметра типа и расширением URL-адреса для подключения как Первый аргумент.</span><span class="sxs-lookup"><span data-stu-id="89f73-336">`MapSignalR` called without parameters will map all hubs (as `MapHubs` does in version 1.x); to map individual **PersistentConnection** objects, specify the connection type as the type parameter, and the URL extension for the connection as the first argument.</span></span>

<span data-ttu-id="89f73-337">`MapSignalR` Метод вызывается в классе запуска Owin.</span><span class="sxs-lookup"><span data-stu-id="89f73-337">The `MapSignalR` method is called in an Owin startup class.</span></span> <span data-ttu-id="89f73-338">Visual Studio 2013 содержит новый шаблон для запуска Owin класса; Чтобы использовать этот шаблон, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="89f73-338">Visual Studio 2013 contains a new template for an Owin startup class; to use this template, do the following:</span></span>

1. <span data-ttu-id="89f73-339">Щелкните правой кнопкой мыши по проекту</span><span class="sxs-lookup"><span data-stu-id="89f73-339">Right-click on the project</span></span>
2. <span data-ttu-id="89f73-340">Выберите **добавить**, **новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="89f73-340">Select **Add**, **New Item...**</span></span>
3. <span data-ttu-id="89f73-341">Выберите **запуска Owin класса**.</span><span class="sxs-lookup"><span data-stu-id="89f73-341">Select **Owin Startup class**.</span></span> <span data-ttu-id="89f73-342">Имя для нового класса **файла Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="89f73-342">Name the new class **Startup.cs**.</span></span>

<span data-ttu-id="89f73-343">В **веб-приложение,** Owin при запуске класса, содержащего `MapSignalR` метод добавляется процесса запуска Owin с помощью записи в узле параметры приложения в файле Web.Config, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="89f73-343">In a **Web application,** the Owin startup class containing the `MapSignalR` method is then added to Owin's startup process using an entry in the application settings node of the Web.Config file, as shown below.</span></span>

<span data-ttu-id="89f73-344">В **резидентных приложений**, класс Startup передается как параметр типа `WebApp.Start` метод.</span><span class="sxs-lookup"><span data-stu-id="89f73-344">In a **Self-hosted application**, the Startup class is passed as the type parameter of the `WebApp.Start` method.</span></span>

<span data-ttu-id="89f73-345">**Сопоставление концентраторов и соединения в SignalR 1.x (из файла глобальных приложения для веб-приложения):**</span><span class="sxs-lookup"><span data-stu-id="89f73-345">**Mapping hubs and connections in SignalR 1.x (from the global application file in a web application):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

<span data-ttu-id="89f73-346">**Сопоставления концентраторов и соединения в SignalR 2.0 (из файла класса запуска Owin):**</span><span class="sxs-lookup"><span data-stu-id="89f73-346">**Mapping hubs and connections in SignalR 2.0 (from an Owin Startup class file):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

<span data-ttu-id="89f73-347">В **резидентных приложений**, класс Startup передается как параметр типа для `WebApp.Start` метода, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="89f73-347">In a **Self-hosted application**, the Startup class is passed as the type parameter for the `WebApp.Start` method, as shown below.</span></span>

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a><span data-ttu-id="89f73-348">Поддержка между доменами</span><span class="sxs-lookup"><span data-stu-id="89f73-348">Cross-Domain Support</span></span>

<span data-ttu-id="89f73-349">В SignalR 1.x междоменные запросы были управляет один флаг EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="89f73-349">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="89f73-350">Этот флаг управлять запросов JSONP и CORS.</span><span class="sxs-lookup"><span data-stu-id="89f73-350">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="89f73-351">Для повышения гибкости, поддерживают все CORS был удален из серверного компонента SignalR (JavaScript lients по-прежнему использовать CORS обычно при его обнаружении, что он поддерживает браузер), и новый по промежуточного слоя OWIN был сделан доступным для поддержки следующих сценариев.</span><span class="sxs-lookup"><span data-stu-id="89f73-351">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript lients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="89f73-352">В версии 2.0 SignalR JSONP, если требуется на клиентском компьютере (для поддержки запросов между доменами в старых браузерах), его необходимо явно включить, установив `EnableJSONP` на `HubConfiguration` объект `true`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="89f73-352">In SignalR 2.0, If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="89f73-353">JSONP отключен по умолчанию, поскольку он менее безопасен, чем CORS.</span><span class="sxs-lookup"><span data-stu-id="89f73-353">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="89f73-354">Чтобы добавить новый по промежуточного слоя CORS в SignalR 2.0, добавьте `Microsoft.Owin.Cors` библиотеки в проекте и вызовите `UseCors` перед SignalR по промежуточного слоя, как показано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="89f73-354">To add the new CORS middleware in SignalR 2.0, add the `Microsoft.Owin.Cors` library to your project, and call `UseCors` before your SignalR middleware, as shown in the section below.</span></span>

<span data-ttu-id="89f73-355">**Добавление в проект Microsoft.Owin.Cors**: чтобы установить эту библиотеку, выполните следующую команду в консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="89f73-355">**Adding Microsoft.Owin.Cors to your project**: To install this library, run the following command in the Package Manager Console:</span></span>

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

<span data-ttu-id="89f73-356">Эта команда добавит 2.0.0 версию пакета для проекта.</span><span class="sxs-lookup"><span data-stu-id="89f73-356">This command will add the 2.0.0 version of the package to your project.</span></span>

<span data-ttu-id="89f73-357">**Вызов UseCors**</span><span class="sxs-lookup"><span data-stu-id="89f73-357">**Calling UseCors**</span></span>

<span data-ttu-id="89f73-358">Следующие фрагменты кода демонстрируют способы реализации подключений между доменами в SignalR 1.x и 2.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-358">The following code snippets demonstrate how to implement cross-domain connections in SignalR 1.x and 2.0.</span></span>

<span data-ttu-id="89f73-359">**Реализация запросов между доменами в SignalR 1.x (из файла глобальные приложения)**</span><span class="sxs-lookup"><span data-stu-id="89f73-359">**Implementing cross-domain requests in SignalR 1.x (from the global application file)**</span></span>

[!code-csharp[Main](release-notes/samples/sample9.cs)]

<span data-ttu-id="89f73-360">**Реализация запросов между доменами в SignalR 2.0 (из файла кода C#)**</span><span class="sxs-lookup"><span data-stu-id="89f73-360">**Implementing cross-domain requests in SignalR 2.0 (from a C# code file)**</span></span>

<span data-ttu-id="89f73-361">Следующий код демонстрирует включение CORS или JSONP в проекте SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-361">The following code demonstrates how to enable CORS or JSONP in a SignalR 2.0 project.</span></span> <span data-ttu-id="89f73-362">Этот пример кода использует `Map` и `RunSignalR` вместо `MapSignalR`, после чего выполняется по промежуточного слоя CORS только для запросов SignalR, которым требуется поддержка CORS (а не для всех типов трафика по пути, указанному в `MapSignalR`.) `Map` также может использоваться для любого другого по промежуточного слоя, необходимо запустить для указанного префикса URL-адрес, а не для всего приложения.</span><span class="sxs-lookup"><span data-stu-id="89f73-362">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) `Map` can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a><span data-ttu-id="89f73-363">iOS и Android, поддерживают через MonoTouch и MonoDroid</span><span class="sxs-lookup"><span data-stu-id="89f73-363">iOS and Android support via MonoTouch and MonoDroid</span></span>

<span data-ttu-id="89f73-364">Добавлена поддержка для iOS и Android клиенты, использующие компоненты MonoTouch и MonoDroid [Xamarin библиотеки](https://xamarin.com/).</span><span class="sxs-lookup"><span data-stu-id="89f73-364">Support has been added for iOS and Android clients using MonoTouch and MonoDroid components from the [Xamarin library](https://xamarin.com/).</span></span> <span data-ttu-id="89f73-365">Дополнительные сведения о способах их использования см. в разделе [компонентов с помощью Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span><span class="sxs-lookup"><span data-stu-id="89f73-365">For more information on how to use them, see [Using Xamarin Components](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span></span> <span data-ttu-id="89f73-366">Эти компоненты будут доступны в [хранилище Xamarin](https://store.xamarin.com/) когда доступна в выпуске SignalR RTW.</span><span class="sxs-lookup"><span data-stu-id="89f73-366">These components will be available in the [Xamarin Store](https://store.xamarin.com/) when the SignalR RTW release is available.</span></span>

<a id="portable"></a><span data-ttu-id="89f73-367">### Портативный клиент .NET</span><span class="sxs-lookup"><span data-stu-id="89f73-367">### Portable .NET client</span></span>

<span data-ttu-id="89f73-368">Для того, чтобы упростить кросс платформенной разработки, Silverlight, WinRT и клиенты Windows Phone были заменены один портативный клиент .NET, поддерживает следующие платформы:</span><span class="sxs-lookup"><span data-stu-id="89f73-368">To better facilitate cross-platform development, the Silverlight, WinRT and Windows Phone clients have been replaced with a single portable .NET client that supports the following platforms:</span></span>

- <span data-ttu-id="89f73-369">NET 4.5</span><span class="sxs-lookup"><span data-stu-id="89f73-369">NET 4.5</span></span>
- <span data-ttu-id="89f73-370">Silverlight 5</span><span class="sxs-lookup"><span data-stu-id="89f73-370">Silverlight 5</span></span>
- <span data-ttu-id="89f73-371">WinRT (.NET для магазина Windows)</span><span class="sxs-lookup"><span data-stu-id="89f73-371">WinRT (.NET for Windows Store Apps)</span></span>
- <span data-ttu-id="89f73-372">Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="89f73-372">Windows Phone 8</span></span>

<a id="selfhost"></a>

### <a name="new-self-host-package"></a><span data-ttu-id="89f73-373">Новый пакет резидентной</span><span class="sxs-lookup"><span data-stu-id="89f73-373">New Self-Host Package</span></span>

<span data-ttu-id="89f73-374">Теперь имеется пакет NuGet для облегчения Приступая к работе с SignalR самостоятельного размещения (SignalR приложений, размещенных в процессе или другие приложения, а не, размещенный на веб-сервере).</span><span class="sxs-lookup"><span data-stu-id="89f73-374">There is now a NuGet package to make it easier to get started with SignalR Self-Host (SignalR applications that are hosted in a process or other application, rather than being hosted in a web server).</span></span> <span data-ttu-id="89f73-375">Для обновления резидентной проекта, созданного с помощью SignalR 1.x, удалите пакет Microsoft.AspNet.SignalR.Owin и добавьте пакет Microsoft.AspNet.SignalR.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="89f73-375">To upgrade a self-host project built with SignalR 1.x, remove the Microsoft.AspNet.SignalR.Owin package, and add the Microsoft.AspNet.SignalR.SelfHost package.</span></span> <span data-ttu-id="89f73-376">Дополнительные сведения о начале работы с резидентной пакета см. в разделе [учебника: SignalR резидентной](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-376">For more information on getting started with the self-host package, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a><span data-ttu-id="89f73-377">Поддержка сервера с обратной совместимостью</span><span class="sxs-lookup"><span data-stu-id="89f73-377">Backward-compatible server support</span></span>

<span data-ttu-id="89f73-378">В предыдущих версиях SignalR, версии SignalR пакета, который используется в клиенте и сервере, должны быть идентичными.</span><span class="sxs-lookup"><span data-stu-id="89f73-378">In previous versions of SignalR, the versions of the SignalR package used in the client and the server needed to be identical.</span></span> <span data-ttu-id="89f73-379">Чтобы обеспечить поддержку толстая клиентские приложения, которые будет трудно обновлять, SignalR 2.0 поддерживает с помощью более новой версии сервера с более старого клиента.</span><span class="sxs-lookup"><span data-stu-id="89f73-379">In order to support thick-client applications that would be difficult to update, SignalR 2.0 now supports using a newer server version with an older client.</span></span> <span data-ttu-id="89f73-380">**Примечание: SignalR 2.0 не поддерживает серверы, построенные с более старыми версиями с помощью новых клиентов.**</span><span class="sxs-lookup"><span data-stu-id="89f73-380">**Note: SignalR 2.0 does not support servers built with older versions with newer clients.**</span></span>

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a><span data-ttu-id="89f73-381">Удалена поддержка сервера .NET 4.0</span><span class="sxs-lookup"><span data-stu-id="89f73-381">Removed server support for .NET 4.0</span></span>

<span data-ttu-id="89f73-382">SignalR 2.0 удалена поддержка сервера взаимодействия с платформой .NET 4.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-382">SignalR 2.0 has dropped support for server interoperability with .NET 4.0.</span></span> <span data-ttu-id="89f73-383">.NET 4.5 необходимо использовать с серверами SignalR 2.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-383">.NET 4.5 must be used with SignalR 2.0 servers.</span></span> <span data-ttu-id="89f73-384">По-прежнему клиент для SignalR 2.0 .NET 4.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-384">There is still a .NET 4.0 client for SignalR 2.0.</span></span>

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a><span data-ttu-id="89f73-385">Отправка сообщения в список клиентов и групп</span><span class="sxs-lookup"><span data-stu-id="89f73-385">Sending a message to a list of clients and groups</span></span>

<span data-ttu-id="89f73-386">В версии 2.0 SignalR можно отправить сообщение, используя список клиента и идентификатор группы.</span><span class="sxs-lookup"><span data-stu-id="89f73-386">In SignalR 2.0, it's possible to send a message using a list of client and group IDs.</span></span> <span data-ttu-id="89f73-387">В следующих фрагментах кода показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="89f73-387">The following code snippets demonstrate how to do this.</span></span>

<span data-ttu-id="89f73-388">**Отправка сообщения в список клиентов и групп с помощью подключение PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="89f73-388">**Sending a message to a list of clients and groups using PersistentConnection**</span></span>

[!code-csharp[Main](release-notes/samples/sample11.cs)]

<span data-ttu-id="89f73-389">**Отправка сообщения в список клиентов и групп с помощью концентраторов**</span><span class="sxs-lookup"><span data-stu-id="89f73-389">**Sending a message to a list of clients and groups using Hubs**</span></span>

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a><span data-ttu-id="89f73-390">Отправка сообщения для определенного пользователя</span><span class="sxs-lookup"><span data-stu-id="89f73-390">Sending a message to a specific user</span></span>

<span data-ttu-id="89f73-391">Эта функция позволяет пользователям указать идентификатор пользователя — в зависимости от IRequest через новый интерфейс IUserIdProvider:</span><span class="sxs-lookup"><span data-stu-id="89f73-391">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider:</span></span>

<span data-ttu-id="89f73-392">**Интерфейс IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="89f73-392">**The IUserIdProvider interface**</span></span>

[!code-csharp[Main](release-notes/samples/sample13.cs)]

<span data-ttu-id="89f73-393">По умолчанию будет реализация, которая использует IPrincipal.Identity.Name пользователя в качестве имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="89f73-393">By default there will be an implementation that uses the user's IPrincipal.Identity.Name as the user name.</span></span>

<span data-ttu-id="89f73-394">В концентраторы можно будет отправлять сообщения для этих пользователей с помощью нового API:</span><span class="sxs-lookup"><span data-stu-id="89f73-394">In hubs, you'll be able to send messages to these users via a new API:</span></span>

<span data-ttu-id="89f73-395">**С помощью Clients.User API**</span><span class="sxs-lookup"><span data-stu-id="89f73-395">**Using the Clients.User API**</span></span>

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a><span data-ttu-id="89f73-396">Улучшенная поддержка обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="89f73-396">Better Error Handling Support</span></span>

<span data-ttu-id="89f73-397">Пользователи теперь могут создавать **HubException** из любого вызова концентратора.</span><span class="sxs-lookup"><span data-stu-id="89f73-397">Users can now throw **HubException** from any hub invocation.</span></span> <span data-ttu-id="89f73-398">Конструктор **HubException** может принимать строковое сообщение и объект дополнительные данные ошибки.</span><span class="sxs-lookup"><span data-stu-id="89f73-398">The constructor of the **HubException** can take a string message and an object extra error data.</span></span> <span data-ttu-id="89f73-399">SignalR будет автоматически сериализации исключения и отправить его клиенту, где он будет использоваться для отклонения или неудача вызова метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="89f73-399">SignalR will auto-serialize the exception and send it to the client where it will be used to reject/fail the hub method invocation.</span></span>

<span data-ttu-id="89f73-400">**Показывать подробные концентратора исключения** параметр никак не влияет **HubException** отправки обратно клиенту или нет; он всегда отправляется.</span><span class="sxs-lookup"><span data-stu-id="89f73-400">The **show detailed hub exceptions** setting has no bearing on **HubException** being sent back to the client or not; it is always sent.</span></span>

<span data-ttu-id="89f73-401">**Демонстрация отправки клиенту HubException серверный код**</span><span class="sxs-lookup"><span data-stu-id="89f73-401">**Server-side code demonstrating sending a HubException to the client**</span></span>

[!code-csharp[Main](release-notes/samples/sample15.cs)]

<span data-ttu-id="89f73-402">**Клиентский код JavaScript, демонстрирующий отвечает на HubException, отправленных с сервера**</span><span class="sxs-lookup"><span data-stu-id="89f73-402">**JavaScript client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-html[Main](release-notes/samples/sample16.html)]

<span data-ttu-id="89f73-403">**.NET клиентского кода, демонстрирующий отвечает на HubException, отправленных с сервера**</span><span class="sxs-lookup"><span data-stu-id="89f73-403">**.NET client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a><span data-ttu-id="89f73-404">Проще модульного тестирования концентраторов</span><span class="sxs-lookup"><span data-stu-id="89f73-404">Easier unit testing of hubs</span></span>

<span data-ttu-id="89f73-405">SignalR 2.0 включает интерфейс `IHubCallerConnectionContext` на концентраторы, упрощает создание макетов клиента стороны вызовов.</span><span class="sxs-lookup"><span data-stu-id="89f73-405">SignalR 2.0 includes an interface called `IHubCallerConnectionContext` on Hubs that makes it easier to create mock client side invocations.</span></span> <span data-ttu-id="89f73-406">Следующие фрагменты кода демонстрируют использование этот интерфейс с популярными тестовой оснастки [xUnit.net](https://github.com/xunit/xunit) и [заказа](https://code.google.com/p/moq/).</span><span class="sxs-lookup"><span data-stu-id="89f73-406">The following code snippets demonstrate using this interface with popular test harnesses [xUnit.net](https://github.com/xunit/xunit) and [moq](https://code.google.com/p/moq/).</span></span>

<span data-ttu-id="89f73-407">**Модульное тестирование с xUnit.net SignalR**</span><span class="sxs-lookup"><span data-stu-id="89f73-407">**Unit testing SignalR with xUnit.net**</span></span>

[!code-csharp[Main](release-notes/samples/sample18.cs)]

<span data-ttu-id="89f73-408">**Модульное тестирование SignalR с заказа**</span><span class="sxs-lookup"><span data-stu-id="89f73-408">**Unit testing SignalR with moq**</span></span>

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a><span data-ttu-id="89f73-409">Обработка ошибок JavaScript</span><span class="sxs-lookup"><span data-stu-id="89f73-409">JavaScript error handling</span></span>

<span data-ttu-id="89f73-410">В версии 2.0 SignalR все обратные вызовы для обработки ошибок JavaScript возвращают объекты ошибки JavaScript вместо необработанных строк.</span><span class="sxs-lookup"><span data-stu-id="89f73-410">In SignalR 2.0, all JavaScript error handling callbacks return JavaScript error objects instead of raw strings.</span></span> <span data-ttu-id="89f73-411">Это позволяет SignalR для потока более подробная информация об для обработчиков ошибок.</span><span class="sxs-lookup"><span data-stu-id="89f73-411">This allows SignalR to flow richer information to your error handlers.</span></span> <span data-ttu-id="89f73-412">Можно получить во внутреннем исключении из `source` свойства ошибки.</span><span class="sxs-lookup"><span data-stu-id="89f73-412">You can get the inner exception from the `source` property of the error.</span></span>

<span data-ttu-id="89f73-413">**Клиентский код JavaScript, который обрабатывает исключение Start.Fail**</span><span class="sxs-lookup"><span data-stu-id="89f73-413">**JavaScript client code that handles the Start.Fail exception**</span></span>

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a><span data-ttu-id="89f73-414">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="89f73-414">ASP.NET Identity</span></span>

### <a name="new-aspnet-membership-system"></a><span data-ttu-id="89f73-415">Новую систему членства ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-415">New ASP.NET Membership System</span></span>

<span data-ttu-id="89f73-416">ASP.NET Identity является систему членства для приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-416">ASP.NET Identity is the new membership system for ASP.NET applications.</span></span> <span data-ttu-id="89f73-417">ASP.NET Identity позволяет легко интегрировать данные профиля конкретного пользователя с данными приложения.</span><span class="sxs-lookup"><span data-stu-id="89f73-417">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="89f73-418">ASP.NET Identity также позволяет выбрать модель сохраняемости для профилей пользователей в приложении.</span><span class="sxs-lookup"><span data-stu-id="89f73-418">ASP.NET Identity also allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="89f73-419">Данные можно сохранить в базе данных SQL Server или другом хранилище данных, включая хранилищ данных NoSQL, таких как таблицы хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="89f73-419">You can store the data in a SQL Server database or another data store, including NoSQL data stores such as Azure Storage Tables.</span></span> <span data-ttu-id="89f73-420">Дополнительные сведения см. в разделе [отдельных учетных записей пользователей](creating-web-projects-in-visual-studio.md#indauth) в **Создание веб-проектов ASP.NET в Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="89f73-420">For more information, see [Individual User Accounts](creating-web-projects-in-visual-studio.md#indauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="claims-based-authentication"></a><span data-ttu-id="89f73-421">Проверка подлинности на основе утверждений</span><span class="sxs-lookup"><span data-stu-id="89f73-421">Claims-based authentication</span></span>

<span data-ttu-id="89f73-422">ASP.NET теперь поддерживает утверждения проверки подлинности на основе которой удостоверение пользователя представляется как набор утверждений от доверенного издателя.</span><span class="sxs-lookup"><span data-stu-id="89f73-422">ASP.NET now supports claims-based authentication, where the user's identity is represented as a set of claims from a trusted issuer.</span></span> <span data-ttu-id="89f73-423">Пользователи могут быть прошедшего проверку подлинности с использованием имени пользователя и пароля, сохраняется в базе данных приложения, или Поставщики удостоверений из социальных сетей (например: учетные записи Microsoft, Facebook, Google, Twitter), или с учетными записями организации через Azure Active Directory или Службы федерации Active Directory (ADFS).</span><span class="sxs-lookup"><span data-stu-id="89f73-423">Users can be authenticated using a username and password maintained in an application database, or using social identity providers (for example: Microsoft Accounts, Facebook, Google, Twitter), or using organizational accounts through Azure Active Directory or Active Directory Federation Services (ADFS).</span></span>

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a><span data-ttu-id="89f73-424">Интеграция с Azure Active Directory и Windows Server Active Directory</span><span class="sxs-lookup"><span data-stu-id="89f73-424">Integration with Azure Active Directory and Windows Server Active Directory</span></span>

<span data-ttu-id="89f73-425">Теперь можно создать проекты ASP.NET, использующих Azure Active Directory или Windows Server Active Directory (AD) для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="89f73-425">You can now create ASP.NET projects that use Azure Active Directory or Windows Server Active Directory (AD) for authentication.</span></span> <span data-ttu-id="89f73-426">Дополнительные сведения см. в разделе [учетные записи организации](creating-web-projects-in-visual-studio.md#orgauth) в **Создание веб-проектов ASP.NET в Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="89f73-426">For more information, see [Organizational Accounts](creating-web-projects-in-visual-studio.md#orgauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="89f73-427">Интеграция OWIN</span><span class="sxs-lookup"><span data-stu-id="89f73-427">OWIN Integration</span></span>

<span data-ttu-id="89f73-428">Проверка подлинности ASP.NET теперь основан на по промежуточного слоя OWIN, который можно использовать на любом узле на основе OWIN.</span><span class="sxs-lookup"><span data-stu-id="89f73-428">ASP.NET authentication is now based on OWIN middleware that can be used on any OWIN-based host.</span></span> <span data-ttu-id="89f73-429">Дополнительные сведения о OWIN см. в следующих [компоненты Microsoft OWIN](#TOC7) раздела.</span><span class="sxs-lookup"><span data-stu-id="89f73-429">For more information about OWIN, see the following [Microsoft OWIN Components](#TOC7) section.</span></span>

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a><span data-ttu-id="89f73-430">Компоненты Microsoft OWIN</span><span class="sxs-lookup"><span data-stu-id="89f73-430">Microsoft OWIN Components</span></span>

<span data-ttu-id="89f73-431">[Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="89f73-431">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="89f73-432">OWIN разрывает связь веб-приложения с сервера, выполняющего web приложений независимой от размещения.</span><span class="sxs-lookup"><span data-stu-id="89f73-432">OWIN decouples the web application from the server, making web applications host-agnostic.</span></span> <span data-ttu-id="89f73-433">Например можно размещения OWIN на основе веб-приложения в IIS или Резидентное размещение в пользовательском процессе.</span><span class="sxs-lookup"><span data-stu-id="89f73-433">For example, you can host an OWIN-based web application in IIS or self-host it in a custom process.</span></span>

<span data-ttu-id="89f73-434">Изменения, появившиеся в компоненты Microsoft OWIN (проект Katana) включать новые компоненты сервера и узел, новые вспомогательные библиотеки и по промежуточного слоя и новый промежуточного по проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="89f73-434">Changes introduced in the Microsoft OWIN components (also known as the Katana project) include new server and host components, new helper libraries and middleware, and new authentication middleware.</span></span>

<span data-ttu-id="89f73-435">Дополнительные сведения об OWIN и Katana см. в разделе [новые возможности OWIN и Katana](../../../aspnet/overview/owin-and-katana/index.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-435">For more information about OWIN and Katana, see [What's new in OWIN and Katana](../../../aspnet/overview/owin-and-katana/index.md).</span></span>

<span data-ttu-id="89f73-436">**Примечание: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) приложения нельзя запускать в классическом режиме IIS; они должны выполняться в режиме интеграции.**</span><span class="sxs-lookup"><span data-stu-id="89f73-436">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications cannot run in IIS classic mode; they must be run in integrated mode.**</span></span>

<span data-ttu-id="89f73-437">**Примечание: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) приложения должны запускаться в режиме полного доверия.**</span><span class="sxs-lookup"><span data-stu-id="89f73-437">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications must be run in full trust.**</span></span>

### <a name="new-servers-and-hosts"></a><span data-ttu-id="89f73-438">Новые серверы и узлы</span><span class="sxs-lookup"><span data-stu-id="89f73-438">New Servers and Hosts</span></span>

<span data-ttu-id="89f73-439">В этом выпуске добавлены новые компоненты, включить сценарии с резидентной.</span><span class="sxs-lookup"><span data-stu-id="89f73-439">With this release, new components were added to enable self-host scenarios.</span></span> <span data-ttu-id="89f73-440">Эти компоненты включают следующие пакеты NuGet:</span><span class="sxs-lookup"><span data-stu-id="89f73-440">These components include the following NuGet packages:</span></span>

- <span data-ttu-id="89f73-441">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="89f73-441">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="89f73-442">Обеспечивает работу сервера OWIN, который использует **HttpListener** для прослушивания HTTP-запросы и направлять их в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="89f73-442">Provides an OWIN server that uses **HttpListener** to listen for HTTP requests and direct them into the OWIN pipeline.</span></span>
- <span data-ttu-id="89f73-443">**Microsoft.Owin.Hosting** представляет собой библиотеку для разработчиков, которые хотят резидентной конвейер OWIN в пользовательском процессе, например консольное приложение или служба Windows.</span><span class="sxs-lookup"><span data-stu-id="89f73-443">**Microsoft.Owin.Hosting** Provides a library for developers who wish to self-host an OWIN pipeline in a custom process, such as a console application or Windows service.</span></span>
- <span data-ttu-id="89f73-444">**OwinHost**.</span><span class="sxs-lookup"><span data-stu-id="89f73-444">**OwinHost**.</span></span> <span data-ttu-id="89f73-445">Предоставляет отдельный исполняемый файл, который создает оболочку для `Microsoft.Owin.Hosting` и позволяет резидентной конвейер OWIN без необходимости написания пользовательское основное приложение.</span><span class="sxs-lookup"><span data-stu-id="89f73-445">Provides a stand-alone executable that wraps `Microsoft.Owin.Hosting` and lets you self-host an OWIN pipeline without having to write a custom host application.</span></span>

<span data-ttu-id="89f73-446">Кроме того `Microsoft.Owin.Host.SystemWeb` пакет теперь позволяет по промежуточного слоя для создания подсказок для **SystemWeb** сервера, указывающее, что по промежуточного слоя должен быть вызван во время определенных этап конвейера ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-446">In addition, the `Microsoft.Owin.Host.SystemWeb` package now enables middleware to provide hints to the **SystemWeb** server, indicating that the middleware should be called during a specific ASP.NET pipeline stage.</span></span> <span data-ttu-id="89f73-447">Эта функция особенно полезна для промежуточного по проверки подлинности, который следует выполнять на ранних стадиях конвейера ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="89f73-447">This feature is particularly useful for authentication middleware, which should run early in the ASP.NET pipeline.</span></span>

### <a name="helper-libraries-and-middleware"></a><span data-ttu-id="89f73-448">Вспомогательные библиотеки и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="89f73-448">Helper Libraries and Middleware</span></span>

<span data-ttu-id="89f73-449">Несмотря на то, что можно написать компонентами OWIN, с помощью только функции и тип определения из спецификации OWIN новый `Microsoft.Owin` пакет предоставляет более удобный набор абстрактные классы.</span><span class="sxs-lookup"><span data-stu-id="89f73-449">Although you can write OWIN components using only the function and type definitions from the OWIN specification, the new `Microsoft.Owin` package provides a more user-friendly set of abstractions.</span></span> <span data-ttu-id="89f73-450">Этот пакет объединяет несколько более ранних пакетов (например, `Owin.Extensions`, `Owin.Types`) в один, хорошо структурированных объектную модель, можно затем легко использовать с другими компонентами OWIN.</span><span class="sxs-lookup"><span data-stu-id="89f73-450">This package combines several earlier packages (e.g., `Owin.Extensions`, `Owin.Types`) into a single, well-structured object model that can then be easily used by other OWIN components.</span></span> <span data-ttu-id="89f73-451">На самом деле этот пакет теперь использовать большую часть компоненты Microsoft OWIN.</span><span class="sxs-lookup"><span data-stu-id="89f73-451">In fact, the majority of Microsoft OWIN components now use this package.</span></span>

> [!NOTE]
> <span data-ttu-id="89f73-452">[OWIN](http://www.owin.org) приложения нельзя запускать в классическом режиме IIS; они должны выполняться в режиме интеграции.</span><span class="sxs-lookup"><span data-stu-id="89f73-452">[OWIN](http://www.owin.org) applications cannot run in IIS classic mode; they must be run in integrated mode.</span></span>

> [!NOTE]
> <span data-ttu-id="89f73-453">[OWIN](http://www.owin.org) приложения должны запускаться в режиме полного доверия.</span><span class="sxs-lookup"><span data-stu-id="89f73-453">[OWIN](http://www.owin.org) applications must be run in full trust.</span></span>

<span data-ttu-id="89f73-454">Этот выпуск также включает пакета Microsoft.Owin.Diagnostics, который включает по промежуточного слоя для проверки приложения OWIN, а также страницу ошибки по промежуточного слоя для анализа сбоев.</span><span class="sxs-lookup"><span data-stu-id="89f73-454">This release also includes the Microsoft.Owin.Diagnostics package, which includes middleware to validate a running OWIN application, plus error-page middleware to help investigate failures.</span></span>

### <a name="authentication-components"></a><span data-ttu-id="89f73-455">Компоненты проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="89f73-455">Authentication Components</span></span>

<span data-ttu-id="89f73-456">Доступны следующие компоненты проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="89f73-456">The following authentication components are available.</span></span>

- <span data-ttu-id="89f73-457">**Microsoft.Owin.Security.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="89f73-457">**Microsoft.Owin.Security.ActiveDirectory**.</span></span> <span data-ttu-id="89f73-458">Включает проверку подлинности с помощью локальных или облачных служб.</span><span class="sxs-lookup"><span data-stu-id="89f73-458">Enables authentication using on-premise or cloud-based directory services.</span></span>
- <span data-ttu-id="89f73-459">**Microsoft.Owin.Security.Cookies** включена проверка подлинности с помощью файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="89f73-459">**Microsoft.Owin.Security.Cookies** Enables authentication using cookies.</span></span> <span data-ttu-id="89f73-460">Этот пакет ранее назывался `Microsoft.Owin.Security.Forms`.</span><span class="sxs-lookup"><span data-stu-id="89f73-460">This package was previously named `Microsoft.Owin.Security.Forms`.</span></span>
- <span data-ttu-id="89f73-461">**Microsoft.Owin.Security.Facebook** включена проверка подлинности с помощью Facebook OAuth-службу.</span><span class="sxs-lookup"><span data-stu-id="89f73-461">**Microsoft.Owin.Security.Facebook** Enables authentication using Facebook's OAuth-based service.</span></span>
- <span data-ttu-id="89f73-462">**Microsoft.Owin.Security.Google** включена проверка подлинности с помощью Google OpenID-службу.</span><span class="sxs-lookup"><span data-stu-id="89f73-462">**Microsoft.Owin.Security.Google** Enables authentication using Google's OpenID-based service.</span></span>
- <span data-ttu-id="89f73-463">**Microsoft.Owin.Security.Jwt** включена проверка подлинности с использованием маркеров JWT.</span><span class="sxs-lookup"><span data-stu-id="89f73-463">**Microsoft.Owin.Security.Jwt** Enables authentication using JWT tokens.</span></span>
- <span data-ttu-id="89f73-464">**Microsoft.Owin.Security.MicrosoftAccount** включена проверка подлинности с помощью учетных записей Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="89f73-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span></span>
- <span data-ttu-id="89f73-465">**Microsoft.Owin.Security.OAuth**.</span><span class="sxs-lookup"><span data-stu-id="89f73-465">**Microsoft.Owin.Security.OAuth**.</span></span> <span data-ttu-id="89f73-466">Предоставляет сервера авторизации OAuth, а также по промежуточного слоя для проверки подлинности маркеров носителя.</span><span class="sxs-lookup"><span data-stu-id="89f73-466">Provides an OAuth authorization server as well as middleware for authenticating bearer tokens.</span></span>
- <span data-ttu-id="89f73-467">**Microsoft.Owin.Security.Twitter** включена проверка подлинности с помощью службы на основе OAuth в Twitter.</span><span class="sxs-lookup"><span data-stu-id="89f73-467">**Microsoft.Owin.Security.Twitter** Enables authentication using Twitter's OAuth-based service.</span></span>

<span data-ttu-id="89f73-468">Этот выпуск также включает `Microsoft.Owin.Cors` пакет, который содержит по промежуточного слоя для обработки HTTP-запросов, независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="89f73-468">This release also includes the `Microsoft.Owin.Cors` package, which contains middleware for processing cross-origin HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="89f73-469">Поддержка JWT подписи был удален в окончательной версии Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="89f73-469">Support for JWT signing has been removed in the final version of Visual Studio 2013.</span></span>

<a id="ef6"></a>
## <a name="entity-framework-6"></a><span data-ttu-id="89f73-470">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="89f73-470">Entity Framework 6</span></span>

<span data-ttu-id="89f73-471">Список новых функций и другие изменения в Entity Framework 6 см. в разделе [истории версий Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="89f73-471">For a list of new features and other changes in Entity Framework 6, see [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a><span data-ttu-id="89f73-472">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="89f73-472">ASP.NET Razor 3</span></span>

<span data-ttu-id="89f73-473">ASP.NET Razor 3 включает следующие новые функции:</span><span class="sxs-lookup"><span data-stu-id="89f73-473">ASP.NET Razor 3 includes the following new features:</span></span>

- <span data-ttu-id="89f73-474">Поддержка изменения вкладки.</span><span class="sxs-lookup"><span data-stu-id="89f73-474">Support for Tab editing.</span></span> <span data-ttu-id="89f73-475">Preivously **форматировать документ** команду, автоматически отступов и форматирования в Visual Studio автоматически не работала правильно при использовании **сохранять знаки табуляции** параметр.</span><span class="sxs-lookup"><span data-stu-id="89f73-475">Preivously, the **Format Document** command, auto indenting, and auto formatting in Visual Studio did not work correctly when using the **Keep Tabs** option.</span></span> <span data-ttu-id="89f73-476">Это изменение устраняет Visual Studio, форматирование кода Razor для табуляции.</span><span class="sxs-lookup"><span data-stu-id="89f73-476">This change corrects Visual Studio formatting for Razor code for tab formatting.</span></span>
- <span data-ttu-id="89f73-477">Поддержка правил переопределения URL-адресов при генерации ссылок.</span><span class="sxs-lookup"><span data-stu-id="89f73-477">Support for URL Rewrite rules when generating links.</span></span>
- <span data-ttu-id="89f73-478">Удаление атрибута прозрачный для безопасности.</span><span class="sxs-lookup"><span data-stu-id="89f73-478">Removal of security transparent attribute.</span></span>
 > [!NOTE]
 > <span data-ttu-id="89f73-479">Это является критическим изменением и делает Razor 3 несовместимым с помощью MVC4 и более ранних версий, пока Razor 2 несовместим с MVC5 или сборки, скомпилирована MVC5.</span><span class="sxs-lookup"><span data-stu-id="89f73-479">This is a breaking change, and makes Razor 3 incompatible with MVC4 and earlier, while Razor 2 is incompatible with MVC5 or assemblies compiled against MVC5.</span></span>

<span data-ttu-id="89f73-480">Ошибки Razor 3 в Visual Studio 2013 с предварительных версий можно найти [здесь](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span><span class="sxs-lookup"><span data-stu-id="89f73-480">Razor 3 issues fixed in Visual Studio 2013 from pre-release versions can be found [here](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span></span>

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a><span data-ttu-id="89f73-481">Приостановка приложений ASP.NET</span><span class="sxs-lookup"><span data-stu-id="89f73-481">ASP.NET App Suspend</span></span>

<span data-ttu-id="89f73-482">Приостановка приложений ASP.NET — это изменение игры компонент в .NET Framework 4.5.1, радикально изменяющий Экономическая модель для размещения большого количества сайтов ASP.NET на одном компьютере и взаимодействия с пользователем.</span><span class="sxs-lookup"><span data-stu-id="89f73-482">ASP.NET App Suspend is a game-changing feature in the .NET Framework 4.5.1 that radically changes the user experience and economic model for hosting large numbers of ASP.NET sites on a single machine.</span></span> <span data-ttu-id="89f73-483">Дополнительные сведения см. в разделе [Приостановка приложений ASP.NET — отвечать на запросы общих веб-размещения .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span><span class="sxs-lookup"><span data-stu-id="89f73-483">For more information, see [ASP.NET App Suspend – responsive shared .NET web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span></span>

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="89f73-484">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="89f73-484">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="89f73-485">В этом разделе описываются известные проблемы и критические изменения в ASP.NET и веб-инструменты для Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="89f73-485">This section describes known issues and breaking changes in the ASP.NET and Web Tools for Visual Studio 2013.</span></span>

### <a name="nuget"></a><span data-ttu-id="89f73-486">NuGet</span><span class="sxs-lookup"><span data-stu-id="89f73-486">NuGet</span></span>

- <span data-ttu-id="89f73-487">[Новый пакет восстановления не работает в моно при использовании файла SLN](https://nuget.codeplex.com/workitem/3596) — проблема будет устранена в будущих nuget.exe загрузки и [NuGet.CommandLine пакета](http://www.nuget.org/packages/NuGet.CommandLine/) обновления.</span><span class="sxs-lookup"><span data-stu-id="89f73-487">[New package restore doesn't work on Mono when using SLN file](https://nuget.codeplex.com/workitem/3596) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="89f73-488">[Новый пакет восстановления не работает с проектами Wix](https://nuget.codeplex.com/workitem/3598) — проблема будет устранена в будущих nuget.exe загрузки и [NuGet.CommandLine пакета](http://www.nuget.org/packages/NuGet.CommandLine/) обновления.</span><span class="sxs-lookup"><span data-stu-id="89f73-488">[New package restore doesn't work with Wix projects](https://nuget.codeplex.com/workitem/3598) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="89f73-489">[Автоматическое восстановление пакетов не работает для проектов в папке решения](https://nuget.codeplex.com/workitem/3625) — проблема будет устранена в NuGet 2.8.</span><span class="sxs-lookup"><span data-stu-id="89f73-489">[Automatic Package restore doesn't work for projects under a solution folder](https://nuget.codeplex.com/workitem/3625) – will be fixed in NuGet 2.8.</span></span>

### <a name="aspnet-web-api"></a><span data-ttu-id="89f73-490">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="89f73-490">ASP.NET Web API</span></span>

1. <span data-ttu-id="89f73-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)`не возвращает `IQueryable<T>` всегда, как мы добавили поддержку `$select` и `$expand`.</span><span class="sxs-lookup"><span data-stu-id="89f73-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` doesn't return `IQueryable<T>` always, as we added support for `$select` and `$expand`.</span></span>

    <span data-ttu-id="89f73-492">Наши примеры ранее для `ODataQueryOptions<T>` всегда привести возвращаемое значение из `ApplyTo` для `IQueryable<T>`.</span><span class="sxs-lookup"><span data-stu-id="89f73-492">Our earlier samples for `ODataQueryOptions<T>` always casted the return value from `ApplyTo` to `IQueryable<T>`.</span></span> <span data-ttu-id="89f73-493">Это работает более ранней версии, так как параметры запроса, которые мы поддерживали более ранней версии (`$filter`, `$orderby`, `$skip`, `$top`) не изменить форму запроса.</span><span class="sxs-lookup"><span data-stu-id="89f73-493">This worked earlier because the query options that we supported earlier (`$filter`, `$orderby`, `$skip`, `$top`) do not change the shape of the query.</span></span> <span data-ttu-id="89f73-494">Теперь, когда мы поддерживаем `$select` и `$expand` возвращаемое значение `ApplyTo` не будет `IQueryable<T>` всегда.</span><span class="sxs-lookup"><span data-stu-id="89f73-494">Now that we support `$select` and `$expand` the return value from `ApplyTo` will not be `IQueryable<T>` always.</span></span>

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    <span data-ttu-id="89f73-495">Если вы используете образец кода из более ранней версии, его будут продолжать работать, если клиент не посылает `$select` и `$expand`.</span><span class="sxs-lookup"><span data-stu-id="89f73-495">If you are using the sample code from earlier, it will continue working if the client does not send `$select` and `$expand`.</span></span> <span data-ttu-id="89f73-496">Тем не менее если вы хотите поддерживать `$select` и `$expand` необходимо изменить этот код для этого.</span><span class="sxs-lookup"><span data-stu-id="89f73-496">However, if you wish to support `$select` and `$expand` you have to change that code to this.</span></span>

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. <span data-ttu-id="89f73-497">**Request.Url или RequestContext.Url имеет значение null во время пакетного запроса**</span><span class="sxs-lookup"><span data-stu-id="89f73-497">**Request.Url or RequestContext.Url is null during a batch request**</span></span>

    <span data-ttu-id="89f73-498">В сценарии пакетной обработки **UrlHelper** имеет значение null, если доступ осуществляется из **Request.Url** или **RequestContext.Url**.</span><span class="sxs-lookup"><span data-stu-id="89f73-498">In a batching scenario, **UrlHelper** is null when accessed from **Request.Url** or **RequestContext.Url**.</span></span>

    <span data-ttu-id="89f73-499">Эта проблема здесь в настоящее время отслеживаются: [BatchRequestContext.Url имеет значение null для пакетной обработки запроса](http://aspnetwebstack.codeplex.com/workitem/1301).</span><span class="sxs-lookup"><span data-stu-id="89f73-499">This issue is currently tracked here: [BatchRequestContext.Url is null for batching request](http://aspnetwebstack.codeplex.com/workitem/1301).</span></span>

    <span data-ttu-id="89f73-500">Решение этой проблемы — создать новый экземпляр **UrlHelper**, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="89f73-500">The workaround for this issue is to create a new instance of **UrlHelper**, as in the following example:</span></span>

    <span data-ttu-id="89f73-501">**Создание нового экземпляра UrlHelper**</span><span class="sxs-lookup"><span data-stu-id="89f73-501">**Creating a new instance of UrlHelper**</span></span>

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a><span data-ttu-id="89f73-502">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="89f73-502">ASP.NET MVC</span></span>

1. <span data-ttu-id="89f73-503">При использовании MVC5 и OrgAuth, если имеются представления, которые выполняют проверку AntiForgerToken, часто встречаются следующие ошибки при отправке данных в представление:</span><span class="sxs-lookup"><span data-stu-id="89f73-503">When using MVC5 and OrgAuth, if you have views which do AntiForgerToken validation, you might come across the following error when you post data to the view:</span></span>

    <span data-ttu-id="89f73-504">**Ошибка**:</span><span class="sxs-lookup"><span data-stu-id="89f73-504">**Error**:</span></span>

    <span data-ttu-id="89f73-505">*Ошибка сервера в приложении '/'.*</span><span class="sxs-lookup"><span data-stu-id="89f73-505">*Server Error in '/' Application.*</span></span>

    <span data-ttu-id="89f73-506">*Утверждение типа «http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier» или «http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider» не был помещен в предоставленный ClaimsIdentity. Чтобы включить поддержку токен борьбы с фальсификацией с проверкой подлинности на основе утверждений, убедитесь, что оба этих утверждений в формируемые им экземплярах ClaimsIdentity содержит поставщика утверждений, настроенный. Если с другим типом утверждения использовал поставщика утверждений, настроенный как уникальный идентификатор, его можно настроить, задав статическое свойство AntiForgeryConfig.UniqueClaimTypeIdentifier.*</span><span class="sxs-lookup"><span data-stu-id="89f73-506">*A claim of type 'http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier' or 'http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider' was not present on the provided ClaimsIdentity. To enable anti-forgery token support with claims-based authentication, please verify that the configured claims provider is providing both of these claims on the ClaimsIdentity instances it generates. If the configured claims provider instead uses a different claim type as a unique identifier, it can be configured by setting the static property AntiForgeryConfig.UniqueClaimTypeIdentifier.*</span></span>

    <span data-ttu-id="89f73-507">**Инструкции по решению**:</span><span class="sxs-lookup"><span data-stu-id="89f73-507">**Workaround**:</span></span>

    <span data-ttu-id="89f73-508">В файле Global.asax исправить эту ошибку, добавьте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="89f73-508">Add the following line in Global.asax to fix it:</span></span>

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    <span data-ttu-id="89f73-509">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="89f73-509">This will be fixed for the next release.</span></span>
2. <span data-ttu-id="89f73-510">После обновления приложения MVC4 до MVC5, выполните сборку решения и запустите его.</span><span class="sxs-lookup"><span data-stu-id="89f73-510">After upgrading an MVC4 app to MVC5, build the solution and launch it.</span></span> <span data-ttu-id="89f73-511">Вы увидите следующую ошибку:</span><span class="sxs-lookup"><span data-stu-id="89f73-511">You should see the following error:</span></span>

    <span data-ttu-id="89f73-512">[A] System.Web.WebPages.Razor.Configuration.HostSection не может быть приведен к [B]System.Web.WebPages.Razor.Configuration.HostSection.</span><span class="sxs-lookup"><span data-stu-id="89f73-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span></span> <span data-ttu-id="89f73-513">Типа A исходит от "System.Web.WebPages.Razor, Version = 2.0.0.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35" в контексте «Default» на месте "C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ версии 4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll ".</span><span class="sxs-lookup"><span data-stu-id="89f73-513">Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span></span> <span data-ttu-id="89f73-514">Типа B исходит от "System.Web.WebPages.Razor, версия = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35" в контексте «Default» в расположении "C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll ".</span><span class="sxs-lookup"><span data-stu-id="89f73-514">Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span></span>

    <span data-ttu-id="89f73-515">Чтобы устранить ошибку выше, откройте *все* файлов Web.config (включая те, в папке «представления») в проекте и выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="89f73-515">To fix the above error, open *all* the Web.config files (including the ones in the Views folder) in your project and do the following:</span></span>

    1. <span data-ttu-id="89f73-516">Обновите все экземпляры версии «4.0.0.0» «System.Web.Mvc» до «5.0.0.0».</span><span class="sxs-lookup"><span data-stu-id="89f73-516">Update all occurrences of version "4.0.0.0" of "System.Web.Mvc" to "5.0.0.0".</span></span>
    2. <span data-ttu-id="89f73-517">Изменить все вхождения «2.0.0.0» версии «System.Web.Helpers» &quot;System.Web.WebPages&quot; и &quot;System.Web.WebPages.Razor&quot; для «3.0.0.0»</span><span class="sxs-lookup"><span data-stu-id="89f73-517">Update all occurrences of version "2.0.0.0" of "System.Web.Helpers", &quot;System.Web.WebPages&quot; and &quot;System.Web.WebPages.Razor&quot; to "3.0.0.0"</span></span>

    <span data-ttu-id="89f73-518">Например после внесения изменения привязок сборок должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="89f73-518">For example, after you make the above changes, the assembly bindings should look like this:</span></span>

    [!code-xml[Main](release-notes/samples/sample24.xml)]

    <span data-ttu-id="89f73-519">Сведения об обновлении проектов MVC 4 до MVC 5 см. в разделе [обновление ASP.NET MVC 4 и API веб-проекта ASP.NET MVC 5 и веб-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="89f73-519">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>
3. <span data-ttu-id="89f73-520">При использовании проверки на стороне клиента с ненавязчивой проверки jQuery, сообщения о проверке неверный иногда для HTML-элемент input типа: «number».</span><span class="sxs-lookup"><span data-stu-id="89f73-520">When using client-side validation with jQuery Unobtrusive Validation, the validation message is sometimes incorrect for an HTML input element with type='number'.</span></span> <span data-ttu-id="89f73-521">Ошибка проверки обязательное значение («поле возраста требуется») отображается при вводе недопустимого номера вместо правильного сообщения, что требуется допустимое число.</span><span class="sxs-lookup"><span data-stu-id="89f73-521">The validation error for a required value ("The Age field is required") is shown when an invalid number is entered instead of the correct message that a valid number is required.</span></span>

    <span data-ttu-id="89f73-522">Эта проблема часто встречаются с кодом формирования шаблонов для модели с целым свойством на создание и изменение представления.</span><span class="sxs-lookup"><span data-stu-id="89f73-522">This issue is commonly found with scaffolded code for a model with an integer property on the Create and Edit views.</span></span>

    <span data-ttu-id="89f73-523">Чтобы обойти эту проблему, измените вспомогательного редактора из:</span><span class="sxs-lookup"><span data-stu-id="89f73-523">To work around this issue, change the editor helper from:</span></span>

    `@Html.EditorFor(person => person.Age)`

    <span data-ttu-id="89f73-524">В:</span><span class="sxs-lookup"><span data-stu-id="89f73-524">To:</span></span>

    `@Html.TextBoxFor(person => person.Age)`
4. <span data-ttu-id="89f73-525">ASP.NET MVC 5 больше не поддерживает частичное доверие.</span><span class="sxs-lookup"><span data-stu-id="89f73-525">ASP.NET MVC 5 no longer supports partial trust.</span></span> <span data-ttu-id="89f73-526">Связывание с MVC или WebAPI двоичные файлы проектов необходимо удалить [SecurityTransparent](https://msdn.microsoft.com/en-us/library/system.security.securitytransparentattribute.aspx) атрибута и [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/en-us/library/system.security.allowpartiallytrustedcallersattribute.aspx) атрибута.</span><span class="sxs-lookup"><span data-stu-id="89f73-526">Projects linking to the MVC or WebAPI binaries should remove the [SecurityTransparent](https://msdn.microsoft.com/en-us/library/system.security.securitytransparentattribute.aspx) attribute and the [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/en-us/library/system.security.allowpartiallytrustedcallersattribute.aspx) attribute.</span></span> <span data-ttu-id="89f73-527">Удаление этих атрибутов исключит следующие ошибки компилятора.</span><span class="sxs-lookup"><span data-stu-id="89f73-527">Removing these attributes will eliminate compiler errors such as the following.</span></span>

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > <span data-ttu-id="89f73-528">Обратите внимание, как побочный эффект этого значения можно не может использовать сборки версии 4.0 и 5.0 в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="89f73-528">Note, as a side effect of this you cannot use 4.0 and 5.0 assemblies in the same application.</span></span> <span data-ttu-id="89f73-529">Необходимо обновить все проекты 5.0.</span><span class="sxs-lookup"><span data-stu-id="89f73-529">You need to update all of them to 5.0.</span></span>

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a><span data-ttu-id="89f73-530">Шаблон SPA с Facebook авторизации может вызвать нестабильность в IE, а веб-сайт размещается в зоне интрасети</span><span class="sxs-lookup"><span data-stu-id="89f73-530">SPA Template with Facebook authorization may cause instability in IE while the web site is hosted in intranet zone</span></span>

<span data-ttu-id="89f73-531">Шаблон SPA предоставляет внешние вход с использованием Facebook.</span><span class="sxs-lookup"><span data-stu-id="89f73-531">The SPA template provides external log in with Facebook.</span></span> <span data-ttu-id="89f73-532">Когда проект, созданный с помощью шаблона выполняется локально, вход может привести к IE аварийное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="89f73-532">When the project created with the template is running locally, signing in may cause IE to crash.</span></span>

<span data-ttu-id="89f73-533">Решение:</span><span class="sxs-lookup"><span data-stu-id="89f73-533">Solution:</span></span>

1. <span data-ttu-id="89f73-534">Веб-узел в зоне Интернета; или</span><span class="sxs-lookup"><span data-stu-id="89f73-534">Host the web site in internet zone; or</span></span>

2. <span data-ttu-id="89f73-535">Проверьте сценарий в браузер, отличный от IE.</span><span class="sxs-lookup"><span data-stu-id="89f73-535">Test the scenario in a browser other than IE.</span></span>

### <a name="web-forms-scaffolding"></a><span data-ttu-id="89f73-536">Web Forms формирование шаблонов</span><span class="sxs-lookup"><span data-stu-id="89f73-536">Web Forms Scaffolding</span></span>

<span data-ttu-id="89f73-537">Формирование шаблонов форм был удален из VS2013 и будет доступна в будущем обновлении Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89f73-537">Web Forms Scaffolding has been removed from VS2013 and will be available in a future update to Visual Studio.</span></span> <span data-ttu-id="89f73-538">Тем не менее по-прежнему можно использовать формирование шаблонов в проекте веб-форм, Добавление зависимостей MVC и создание формирование шаблонов для MVC.</span><span class="sxs-lookup"><span data-stu-id="89f73-538">However, you can still use scaffolding within a Web Forms project by adding MVC dependencies and generating scaffolding for MVC.</span></span> <span data-ttu-id="89f73-539">Проект будет содержать сочетание веб-форм и MVC.</span><span class="sxs-lookup"><span data-stu-id="89f73-539">Your project will contain a combination of Web Forms and MVC.</span></span>

<span data-ttu-id="89f73-540">Чтобы добавить MVC в проект веб-форм, добавить новый элемент формирования шаблонов и выберите **зависимостей MVC 5**.</span><span class="sxs-lookup"><span data-stu-id="89f73-540">To add MVC to your Web Forms project, add a new scaffolded item and select **MVC 5 Dependencies**.</span></span> <span data-ttu-id="89f73-541">Выберите минимальный или полный в зависимости от того, нужна ли все файлы содержимого, таких как сценарии.</span><span class="sxs-lookup"><span data-stu-id="89f73-541">Select either Minimal or Full depending on whether you need all of the content files, such as scripts.</span></span> <span data-ttu-id="89f73-542">Затем добавьте элемент формирования шаблонов для MVC, в которой будут созданы представления и контроллера в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="89f73-542">Then, add a scaffolded item for MVC, which will create views and a controller in your project.</span></span>

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="89f73-543">MVC и API формирование шаблонов-HTTP 404 не найдено ошибок</span><span class="sxs-lookup"><span data-stu-id="89f73-543">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="89f73-544">Если возникает ошибка при добавлении элемента формирования шаблонов в проект, возможно, проект может остаться в несогласованном состоянии.</span><span class="sxs-lookup"><span data-stu-id="89f73-544">If an error is encountered when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="89f73-545">Некоторые изменения, внесенные быть формирование шаблонов будет выполнен откат, но другие изменения, например установленные пакеты NuGet, не будет выполнен откат.</span><span class="sxs-lookup"><span data-stu-id="89f73-545">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="89f73-546">Если откат изменения конфигурации маршрутизации, пользователи получат ошибку HTTP 404 при навигации для формирования шаблонов элементов.</span><span class="sxs-lookup"><span data-stu-id="89f73-546">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="89f73-547">Инструкции по решению:</span><span class="sxs-lookup"><span data-stu-id="89f73-547">Workaround:</span></span>

- <span data-ttu-id="89f73-548">Чтобы устранить эту ошибку для MVC, добавить новый элемент формирования шаблонов и выберите зависимостей MVC 5 (минимум или полный).</span><span class="sxs-lookup"><span data-stu-id="89f73-548">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="89f73-549">Этот процесс будет добавить все необходимые изменения в проект.</span><span class="sxs-lookup"><span data-stu-id="89f73-549">This process will add all of the required changes to your project.</span></span>
- <span data-ttu-id="89f73-550">Чтобы устранить эту ошибку для веб-API:</span><span class="sxs-lookup"><span data-stu-id="89f73-550">To fix this error for Web API:</span></span>

    1. <span data-ttu-id="89f73-551">Добавление класса WebApiConfig в проект.</span><span class="sxs-lookup"><span data-stu-id="89f73-551">Add the WebApiConfig class to your project.</span></span>

        [!code-csharp[Main](release-notes/samples/sample25.cs)]

        [!code-vb[Main](release-notes/samples/sample26.vb)]
    2. <span data-ttu-id="89f73-552">Настройка WebApiConfig.Register в приложении\_Start-метод в файле Global.asax следующим образом:</span><span class="sxs-lookup"><span data-stu-id="89f73-552">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

        [!code-csharp[Main](release-notes/samples/sample27.cs)]

        [!code-vb[Main](release-notes/samples/sample28.vb)]
