---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "ASP.NET MVC 4 настраиваемых фильтров действий | Документы Microsoft"
author: rick-anderson
description: "ASP.NET MVC предоставляет фильтры действий для выполнения логику фильтрации до или после вызова метода действия. Фильтры действий представляют собой пользовательские атрибуты tha..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 103cd68c576463d87d0077cc149f9b89c6e028e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="b8fbb-104">ASP.NET MVC 4 настраиваемых фильтров действий</span><span class="sxs-lookup"><span data-stu-id="b8fbb-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="b8fbb-105">по [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b8fbb-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="b8fbb-106">ASP.NET MVC предоставляет фильтры действий для выполнения логику фильтрации до или после вызова метода действия.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="b8fbb-107">Фильтры действий представляют собой пользовательские атрибуты, позволяющие декларативным образом добавлять поведение до и после действия в методы действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="b8fbb-108">В этой лаборатории практических создаст атрибут фильтра настраиваемое действие в MvcMusicStore решение для перехвата запросов контроллера и регистрирует действия узла в таблицу базы данных.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="b8fbb-109">Можно добавить любой контроллер или действие фильтра ведения журнала путем внедрения кода.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="b8fbb-110">Наконец вы увидите представление журнала, где отображается список посетителей.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="b8fbb-111">Это Практическое лабораторное занятие предполагает иметь базовые знания об **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="b8fbb-112">Если вы не использовали **ASP.NET MVC** раньше, мы рекомендуем перейти **ASP.NET MVC 4 основы** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b8fbb-113">Цели</span><span class="sxs-lookup"><span data-stu-id="b8fbb-113">Objectives</span></span>

<span data-ttu-id="b8fbb-114">В этой лаборатории практических вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b8fbb-115">Создайте атрибут фильтра настраиваемого действия, чтобы расширить возможности фильтрации</span><span class="sxs-lookup"><span data-stu-id="b8fbb-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="b8fbb-116">Применение пользовательского атрибута фильтра путем внедрения кода до определенного уровня</span><span class="sxs-lookup"><span data-stu-id="b8fbb-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="b8fbb-117">Регистрация пользовательских фильтров действий глобально</span><span class="sxs-lookup"><span data-stu-id="b8fbb-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b8fbb-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="b8fbb-118">Prerequisites</span></span>

<span data-ttu-id="b8fbb-119">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b8fbb-120">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b8fbb-121">Установка</span><span class="sxs-lookup"><span data-stu-id="b8fbb-121">Setup</span></span>

<span data-ttu-id="b8fbb-122">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="b8fbb-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="b8fbb-123">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b8fbb-124">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b8fbb-125">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b8fbb-126">Упражнения</span><span class="sxs-lookup"><span data-stu-id="b8fbb-126">Exercises</span></span>

<span data-ttu-id="b8fbb-127">Это Практическое лабораторное занятие состоит в выполнении следующих упражнений.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b8fbb-128">Упражнение 1: Ведение журнала действий</span><span class="sxs-lookup"><span data-stu-id="b8fbb-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="b8fbb-129">Упражнение 2: Управление нескольких фильтров</span><span class="sxs-lookup"><span data-stu-id="b8fbb-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="b8fbb-130">Предполагаемое время для выполнения этого занятия: **30 минут**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b8fbb-131">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b8fbb-132">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="b8fbb-133">Упражнение 1: Ведение журнала действий</span><span class="sxs-lookup"><span data-stu-id="b8fbb-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="b8fbb-134">В этом упражнении вы узнаете, как для создания пользовательского журнала фильтра действий с помощью поставщики фильтров ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="b8fbb-135">Для этой цели будет применен фильтр ведения журнала на сайт MusicStore, регистрировать все действия в выбранных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="b8fbb-136">Будет расширить фильтр **ActionFilterAttributeClass** и Переопределите **OnActionExecuting** метод перехватывать каждый запрос, а затем выполнять ведение журнала действий.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="b8fbb-137">Контекстные сведения об HTTP-запросов, выполнение методов, результаты и параметры будут предоставляться ASP.NET MVC **ActionExecutingContext** класса **.**</span><span class="sxs-lookup"><span data-stu-id="b8fbb-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="b8fbb-138">ASP.NET MVC 4 также имеет поставщики фильтров по умолчанию, которые можно использовать без создания пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="b8fbb-139">ASP.NET MVC 4 предоставляет следующие типы фильтров:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="b8fbb-140">**Авторизация** фильтрации, что делает безопасности решений о необходимости выполнения метода действия, такие как выполнение проверки подлинности или проверки свойств запроса.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="b8fbb-141">**Действие** фильтр, который создает оболочку для выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="b8fbb-142">Этот фильтр может выполнять дополнительную обработку, например предоставлять дополнительные данные методу действия, инспектировать возвращаемое значение или отменять выполнение метода действия</span><span class="sxs-lookup"><span data-stu-id="b8fbb-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="b8fbb-143">**Результат** фильтр, который является оболочкой для исполнения объекта ActionResult.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="b8fbb-144">Этот фильтр может выполнять дополнительную обработку результата, например изменить HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="b8fbb-145">**Исключение** фильтр, который выполняется при наличии необработанного исключения в методе действия, начиная с фильтры авторизации и заканчивая выполнением результата.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="b8fbb-146">Фильтры исключений можно использовать для ведения журнала или отображения страниц об ошибках.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="b8fbb-147">Дополнительные сведения о поставщиках фильтры посетите эту ссылку MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="b8fbb-148">О функции ведения журнала хранилища музыка MVC-приложения</span><span class="sxs-lookup"><span data-stu-id="b8fbb-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="b8fbb-149">Это решение Music Store имеет новую таблицу модели данных для ведения журнала для сайта, **ActionLog**, со следующими полями: имя контроллера, который получил запрос, вызываемые действия, IP-адрес клиента и отметки времени.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="b8fbb-150">![Модель данных. Таблица ActionLog. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Модели данных. Таблица ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="b8fbb-151">*Модель данных — ActionLog таблицы*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="b8fbb-152">Решение обеспечивает представление MVC ASP.NET в журнал действий, можно найти в **MvcMusicStores/представления/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="b8fbb-153">![Просмотр журнала действий](aspnet-mvc-4-custom-action-filters/_static/image2.png "представление журнала действий")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="b8fbb-154">*Просмотр журнала действий*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-154">*Action Log view*</span></span>

<span data-ttu-id="b8fbb-155">Таким образом получает структуры вся работа будет фокус ввода на прерывания запроса контроллера и выполнения ведения журнала с помощью пользовательских фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="b8fbb-156">Задача 1. Создание пользовательского фильтра для перехвата запроса контроллера</span><span class="sxs-lookup"><span data-stu-id="b8fbb-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="b8fbb-157">В этой задаче вы создадите класс атрибут пользовательского фильтра, который будет содержать логику занесения данных.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="b8fbb-158">Для этой цели можно расширить ASP.NET MVC **ActionFilterAttribute** класса и реализуйте интерфейс **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="b8fbb-159">**ActionFilterAttribute** является базовым классом для всех атрибутов фильтров.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="b8fbb-160">Он предоставляет следующие методы для выполнения определенной логики после и перед выполнением действия контроллера:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="b8fbb-161">**OnActionExecuting**(ActionExecutingContext filterContext): метод вызывается непосредственно перед действие.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="b8fbb-162">**OnActionExecuted**(ActionExecutedContext filterContext): после вызова метода действия и перед выполнением результат (до отображения представления).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="b8fbb-163">**OnResultExecuting**(ResultExecutingContext filterContext): перед выполняется результат (до отображения представления).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="b8fbb-164">**OnResultExecuted**(ResultExecutedContext filterContext): после выполнения результата (после визуализации представления).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="b8fbb-165">Путем переопределения любого из этих методов в производном классе, выполнением кода фильтрации.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="b8fbb-166">Откройте **начать** решений, расположенный **\Source\Ex01-LoggingActions\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="b8fbb-167">Необходимо будет загрузить несколько отсутствующих пакетов NuGet, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="b8fbb-168">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b8fbb-169">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b8fbb-170">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8fbb-171">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b8fbb-172">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b8fbb-173">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="b8fbb-174">Дополнительные сведения см. в разделе этой статьи: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b8fbb-175">Добавьте новый класс C# в **фильтры** папки и назовите его *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="b8fbb-176">В этой папке будут храниться все настраиваемые фильтры.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="b8fbb-177">Откройте **CustomActionFilter.cs** и добавьте ссылку на **System.Web.Mvc** и **MvcMusicStore.Models** пространства имен:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="b8fbb-178">(Фрагмент - кода *ASP.NET MVC 4 настраиваемых фильтров действий - сервера Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="b8fbb-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="b8fbb-179">Наследовать **CustomActionFilter** класса из **ActionFilterAttribute** и затем **CustomActionFilter** Реализуйте класс **IActionFilter** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="b8fbb-180">Сделать **CustomActionFilter** класс переопределить метод **OnActionExecuting** и добавьте необходимую логику в журнал выполнения фильтра.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="b8fbb-181">Чтобы сделать это, добавьте следующий выделенный код в **CustomActionFilter** класса.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="b8fbb-182">(Фрагмент - кода *ASP.NET MVC 4 настраиваемых фильтров действий - сервера Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="b8fbb-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="b8fbb-183">**OnActionExecuting** использует метод **Entity Framework** для добавления нового ActionLog регистра.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="b8fbb-184">Он создает и заполняет новый экземпляр сущности из сведений о контексте **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="b8fbb-185">Дополнительные сведения о **параметром ControllerContext** класса в [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="b8fbb-186">Задача 2 - Добавление перехватчик код в класс контроллера хранилища</span><span class="sxs-lookup"><span data-stu-id="b8fbb-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="b8fbb-187">В этой задаче вы добавите пользовательский фильтр, внедряя его для всех классов контроллеров и действий контроллера, которые будут регистрироваться.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="b8fbb-188">Для этого упражнения класс контроллера хранилища будут иметь журналов.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="b8fbb-189">Метод **OnActionExecuting** из **ActionLogFilterAttribute** настраиваемого фильтра выполняется при вызове внедренного элемента.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="b8fbb-190">Можно также перехватывать метод конкретному контроллеру.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="b8fbb-191">Откройте **StoreController** в **MvcMusicStore\Controllers** и добавьте ссылку на **фильтры** пространство имен:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="b8fbb-192">Внедрить пользовательский фильтр **CustomActionFilter** в **StoreController** класс, добавив **[CustomActionFilter]** перед объявлением класса.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="b8fbb-193">Если фильтр вставляется в класс контроллера, также добавляются все свои действия.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="b8fbb-194">Если вы хотите применить фильтр только для набора действий, потребовалось бы ввести **[CustomActionFilter]** для каждой из них:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b8fbb-195">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="b8fbb-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="b8fbb-196">В этой задаче вы проверите, что фильтр регистрации работает.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="b8fbb-197">Будет запустите приложение и посетить магазин, и затем производится проверка журнала действий.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="b8fbb-198">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b8fbb-199">Перейдите к **/ActionLog** начальное состояние представления журнала:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="b8fbb-200">![Состояние отслеживания перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image3.png "отслеживания состояния перед действием страницы журнала")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b8fbb-201">*Состояние регистрации журнала до действия "страницы"*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8fbb-202">По умолчанию он всегда будет отображаться один элемент, который создается при получении существующих жанров для меню.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="b8fbb-203">В целях упрощения мы происходит очистка **ActionLog** таблицы при каждом запуске приложения, будут отображаться только журналы проверки каждой конкретной задачи.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="b8fbb-204">Может потребоваться удалить следующий код из **сеанса\_запустить** метода (в **Global.asax** класса), чтобы сохранить журнал для всех действий, которые были выполнены в хранилище Контроллер.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="b8fbb-205">Выберите один из **жанров** из меню и выполнять некоторые действия, такие как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="b8fbb-206">Перейдите к **/ActionLog** и при наличии журнала пустой клавишу **F5** обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="b8fbb-207">Проверьте, что были отслежены посещенных:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="b8fbb-208">![Журнал действий с записываемого действия](aspnet-mvc-4-custom-action-filters/_static/image4.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8fbb-209">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="b8fbb-210">Упражнение 2: Управление нескольких фильтров</span><span class="sxs-lookup"><span data-stu-id="b8fbb-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="b8fbb-211">В этом упражнении будет добавить второй настраиваемый фильтр действий в класс StoreController и определить определенного порядка, в котором будет выполняться оба фильтра.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="b8fbb-212">Затем будет обновить код, чтобы зарегистрировать глобальный фильтр.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="b8fbb-213">Существуют различные варианты необходимо учитывать при определении порядка выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="b8fbb-214">Например свойство Order и области фильтрации:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="b8fbb-215">Можно определить **область** для каждого из фильтров, например, вы может ограничить область видимости все фильтры действий для запуска в составе **область контроллера**и все фильтры авторизации для запуска в **глобальной области видимости** .</span><span class="sxs-lookup"><span data-stu-id="b8fbb-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="b8fbb-216">Области имеют порядок определения выполнение.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="b8fbb-217">Кроме того для каждого фильтра действия есть свойство Order, который используется для определения порядка выполнения в область фильтра.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="b8fbb-218">Дополнительные сведения о порядке выполнения настраиваемые фильтры действий можно найти в этой статье MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="b8fbb-219">Задача 1: Создание нового настраиваемого фильтра действий</span><span class="sxs-lookup"><span data-stu-id="b8fbb-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="b8fbb-220">В этой задаче вы создадите новый настраиваемый фильтр действий для вставки в классе StoreController научиться управлять порядок выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="b8fbb-221">Откройте **начать** решений, расположенный **\Source\Ex02-ManagingMultipleActionFilters\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="b8fbb-222">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b8fbb-223">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b8fbb-224">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b8fbb-225">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b8fbb-226">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="b8fbb-227">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b8fbb-228">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b8fbb-229">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="b8fbb-230">Дополнительные сведения см. в разделе этой статьи: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b8fbb-231">Добавьте новый класс C# в **фильтры** папки и назовите его *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="b8fbb-232">Откройте **MyNewCustomActionFilter.cs** и добавьте ссылку на **System.Web.Mvc** и **MvcMusicStore.Models** пространство имен:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="b8fbb-233">(Фрагмент - кода *ASP.NET MVC 4 настраиваемых фильтров действий - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="b8fbb-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="b8fbb-234">Замените объявление класса по умолчанию с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="b8fbb-235">(Фрагмент - кода *ASP.NET MVC 4 настраиваемых фильтров действий - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="b8fbb-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="b8fbb-236">Этот настраиваемый фильтр действий почти не отличается от той, которую вы создали в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="b8fbb-237">Основное отличие заключается в том, что он имеет  *&quot;входа по&quot;*  атрибута, который обновляется с именем этот новый класс, чтобы определить фильтр, который зарегистрирован в журнале.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="b8fbb-238">Задача 2: Добавление нового перехватчик код в класс StoreController</span><span class="sxs-lookup"><span data-stu-id="b8fbb-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="b8fbb-239">В этой задаче будет добавить в класс StoreController новый настраиваемый фильтр и запустите решение, чтобы проверить, как оба фильтра совместной работы.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="b8fbb-240">Откройте **StoreController** класса, расположенный **MvcMusicStore\Controllers** и вставляют новый настраиваемый фильтр **MyNewCustomActionFilter** в  **StoreController** класса, как показано в следующем коде.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="b8fbb-241">Теперь запустите приложение, чтобы увидеть, как работают эти две настраиваемые фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="b8fbb-242">Чтобы сделать это, нажмите клавишу **F5** и подождите, пока приложение запускается.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="b8fbb-243">Перейдите к **/ActionLog** для просмотра исходного состояния представление журнала.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="b8fbb-244">![Состояние отслеживания перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image5.png "отслеживания состояния перед действием страницы журнала")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b8fbb-245">*Состояние регистрации журнала до действия "страницы"*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="b8fbb-246">Выберите один из **жанров** из меню и выполнять некоторые действия, такие как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="b8fbb-247">Убедитесь, что это время; посещенных отслеживались дважды: после добавления в для каждого из фильтров действий пользовательского **готовности StorageController** класса.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="b8fbb-248">![Журнал действий с записываемого действия](aspnet-mvc-4-custom-action-filters/_static/image6.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8fbb-249">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="b8fbb-250">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="b8fbb-251">Задача 3: Управление упорядочение фильтра</span><span class="sxs-lookup"><span data-stu-id="b8fbb-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="b8fbb-252">В этой задаче рассматривается управление порядок выполнения фильтров с помощью свойства заказа.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="b8fbb-253">Откройте **StoreController** класса, расположенный **MvcMusicStore\Controllers** и укажите **порядок** свойству в оба фильтра, такому как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="b8fbb-254">Теперь проверьте, как фильтры выполняются в зависимости от значения свойства Order.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="b8fbb-255">Вы найдете, фильтр с наименьшим значением (**CustomActionFilter**) является первым, выполняется.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="b8fbb-256">Нажмите клавишу **F5** и подождите, пока приложение запускается.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="b8fbb-257">Перейдите к **/ActionLog** для просмотра исходного состояния представление журнала.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="b8fbb-258">![Состояние отслеживания перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image7.png "отслеживания состояния перед действием страницы журнала")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b8fbb-259">*Состояние регистрации журнала до действия "страницы"*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="b8fbb-260">Выберите один из **жанров** из меню и выполнять некоторые действия, такие как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="b8fbb-261">Проверьте, что на этот раз посещенных отслеживались упорядоченные по фильтрации порядковый номер: **CustomActionFilter** журналы первой.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="b8fbb-262">![Журнал действий с записываемого действия](aspnet-mvc-4-custom-action-filters/_static/image8.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8fbb-263">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="b8fbb-264">Теперь будет обновлять значения порядка фильтров и проверьте, как изменяется порядок ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="b8fbb-265">В **StoreController** класса, обновить значение порядка фильтрации, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="b8fbb-266">Снова запустите приложение, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="b8fbb-267">Выберите один из **жанров** из меню и выполнять некоторые действия, такие как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="b8fbb-268">Проверьте, что на этот раз журналов, созданных **MyNewCustomActionFilter** фильтра отображаются первыми.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="b8fbb-269">![Журнал действий с записываемого действия](aspnet-mvc-4-custom-action-filters/_static/image9.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8fbb-270">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="b8fbb-271">Задача 4: Регистрация фильтрует глобально</span><span class="sxs-lookup"><span data-stu-id="b8fbb-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="b8fbb-272">В этой задаче будет обновить решение, чтобы зарегистрировать новый фильтр (**MyNewCustomActionFilter**) как глобальный.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="b8fbb-273">Таким образом, он будет запускаться все perfomed действия в приложении, а не только в StoreController из них, как в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="b8fbb-274">В **StoreController** класса, удалите **[MyNewCustomActionFilter]** атрибутов и свойств заказа из **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="b8fbb-275">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="b8fbb-276">Откройте **Global.asax** и найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="b8fbb-277">Обратите внимание, что каждый thime приложение запускает его регистрация глобальных фильтров путем вызова **RegisterGlobalFilters** метода в **FilterConfig** класса.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="b8fbb-278">![Регистрация глобальных фильтров в файле Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "регистрация глобальных фильтров в файле Global.asax")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="b8fbb-279">*Регистрация глобальных фильтров в файле Global.asax*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="b8fbb-280">Откройте **FilterConfig.cs** файл включен в **приложения\_запустить** папки.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="b8fbb-281">Добавьте ссылку на System.Web.Mvc; с помощью с помощью MvcMusicStore.Filters; пространство имен.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="b8fbb-282">Обновление **RegisterGlobalFilters** метод добавления пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="b8fbb-283">Чтобы сделать это, добавьте выделенный код:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="b8fbb-284">Запустите приложение, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="b8fbb-285">Выберите один из **жанров** из меню и выполнять некоторые действия, такие как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="b8fbb-286">Проверьте, что теперь **[MyNewCustomActionFilter]** , введенного в HomeController и ActionLogController слишком.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="b8fbb-287">![Журнал действий с записываемого действия](aspnet-mvc-4-custom-action-filters/_static/image11.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8fbb-288">*Журнал действий с глобальной записываемого действия.*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="b8fbb-289">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b8fbb-290">Сводка</span><span class="sxs-lookup"><span data-stu-id="b8fbb-290">Summary</span></span>

<span data-ttu-id="b8fbb-291">Выполнив это Практическое лабораторное занятие изучено расширить фильтр действий для выполнения настраиваемых действий.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="b8fbb-292">Вы также узнали, как внедрить любой фильтр контроллеры страницы.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="b8fbb-293">Были использованы следующие понятия:</span><span class="sxs-lookup"><span data-stu-id="b8fbb-293">The following concepts were used:</span></span>

- <span data-ttu-id="b8fbb-294">Создание настраиваемого действия фильтров в классе ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b8fbb-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="b8fbb-295">Как для добавления фильтров в ASP.NET MVC контроллеры</span><span class="sxs-lookup"><span data-stu-id="b8fbb-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="b8fbb-296">Управление фильтра упорядочение с помощью свойства Order</span><span class="sxs-lookup"><span data-stu-id="b8fbb-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="b8fbb-297">Как зарегистрировать фильтры глобально</span><span class="sxs-lookup"><span data-stu-id="b8fbb-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b8fbb-298">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="b8fbb-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b8fbb-299">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="b8fbb-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b8fbb-300">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b8fbb-301">Последовательно выберите пункты [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b8fbb-302">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; *Visual Studio Express 2012 для Web с пакетом Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="b8fbb-303">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-303">Click on **Install Now**.</span></span> <span data-ttu-id="b8fbb-304">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b8fbb-305">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b8fbb-306">![Установка Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b8fbb-307">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b8fbb-308">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="b8fbb-310">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b8fbb-311">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-311">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="b8fbb-313">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-313">*Installation progress*</span></span>
6. <span data-ttu-id="b8fbb-314">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-314">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="b8fbb-316">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-316">*Installation completed*</span></span>
7. <span data-ttu-id="b8fbb-317">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b8fbb-318">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="b8fbb-320">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b8fbb-321">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="b8fbb-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b8fbb-322">В этом приложении будет показано, как создать новый веб-сайт на портале управления Windows Azure и публикация приложения, получить, выполнив в лаборатории преимуществами средство публикации веб-развертывания, предоставленного Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="b8fbb-323">Задача 1. Создание нового веб-узла с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b8fbb-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="b8fbb-324">Последовательно выберите пункты [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8fbb-325">С помощью Windows Azure можно бесплатно размещения 10 веб-сайтов ASP.NET и затем масштабирование по мере увеличения трафика.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b8fbb-326">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b8fbb-327">![Войдите на портал Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "входа на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b8fbb-328">*Выполните вход на портале управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="b8fbb-329">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b8fbb-330">![Создание нового веб-сайта](aspnet-mvc-4-custom-action-filters/_static/image18.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b8fbb-331">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b8fbb-332">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b8fbb-333">Выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="b8fbb-334">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **Создание веб-сайта**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8fbb-335">Веб-приложение, работающее в облаке, вы можете управлять размещается веб-сайта Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b8fbb-336">Параметр быстрого создания позволяет развернуть завершенное веб-приложение для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="b8fbb-337">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b8fbb-338">![Создание нового веб-сайта с помощью быстрого создания](aspnet-mvc-4-custom-action-filters/_static/image19.png "создания нового веб-сайта с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b8fbb-339">*Создание нового веб-сайта с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b8fbb-340">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b8fbb-341">После создания веб-сайта по ссылке **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b8fbb-342">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b8fbb-343">![Просмотр новых веб-сайт](aspnet-mvc-4-custom-action-filters/_static/image20.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b8fbb-344">*Просмотр новых веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b8fbb-345">![Веб-сайта под управлением](aspnet-mvc-4-custom-action-filters/_static/image21.png "веб-сайта под управлением")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="b8fbb-346">*Запуск веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-346">*Web site running*</span></span>
6. <span data-ttu-id="b8fbb-347">Вернитесь на портал и щелкните имя веб-сайта в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b8fbb-348">![Открытие страницы управления веб-сайт](aspnet-mvc-4-custom-action-filters/_static/image22.png "Открытие страниц управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b8fbb-349">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b8fbb-350">В **панели мониторинга** в разделе **беглого** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8fbb-351">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайте Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="b8fbb-352">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверки подлинности с каждой из конечных точек, для которых включен метода публикации.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b8fbb-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки этих программ для Публикация веб-приложений на веб-сайтов Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="b8fbb-354">![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-custom-action-filters/_static/image23.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b8fbb-355">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b8fbb-356">Загрузите файл профиля публикации в известном расположении.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b8fbb-357">Далее в этом упражнении вы увидите как использовать этот файл для публикации веб-приложения для веб-сайтов, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="b8fbb-358">![Сохранить файл профиля публикации](aspnet-mvc-4-custom-action-filters/_static/image24.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b8fbb-359">*Сохранить файл профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b8fbb-360">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="b8fbb-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b8fbb-361">Если приложение использует SQL Server базы данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b8fbb-362">Если вы хотите развернуть простое приложение, которое не использует SQL Server может пропустить эту задачу.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b8fbb-363">Сервер базы данных SQL необходимо для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b8fbb-364">Можно просматривать серверы базы данных SQL из подписки на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b8fbb-365">Если у вас на сервер, созданный, можно создать ее, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b8fbb-366">Обратите внимание **имя сервера и URL-адрес, имя входа администратора и пароль**, как будет использовать их в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b8fbb-367">Не следует создавать базу данных, как он будет создан в более поздней стадии.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b8fbb-368">![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-custom-action-filters/_static/image25.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b8fbb-369">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b8fbb-370">В следующей задаче требуется проверить подключение к базе данных из Visual Studio, по этой причине необходимо включить локальный IP-адреса в список серверов **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b8fbb-371">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Добавление IP-адрес клиента](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="b8fbb-373">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b8fbb-374">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="b8fbb-376">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b8fbb-377">Задача 3. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="b8fbb-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b8fbb-378">Вернитесь в решении ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b8fbb-379">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b8fbb-380">![Публикация приложения](aspnet-mvc-4-custom-action-filters/_static/image29.png "публикация приложения")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="b8fbb-381">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="b8fbb-382">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b8fbb-383">![Импорт профиля публикации](aspnet-mvc-4-custom-action-filters/_static/image30.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b8fbb-384">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="b8fbb-385">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-385">Click **Validate Connection**.</span></span> <span data-ttu-id="b8fbb-386">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8fbb-387">Проверка завершена, как только вы увидите зеленый флажок рядом с "Проверить подключение".</span><span class="sxs-lookup"><span data-stu-id="b8fbb-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b8fbb-388">![Проверка подключения](aspnet-mvc-4-custom-action-filters/_static/image31.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="b8fbb-389">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-389">*Validating connection*</span></span>
4. <span data-ttu-id="b8fbb-390">В **параметры** в разделе **баз данных** статьи, нажмите кнопку рядом с текстового поля для подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b8fbb-391">![Конфигурация веб-развертывания](aspnet-mvc-4-custom-action-filters/_static/image32.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b8fbb-392">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b8fbb-393">Настройте подключение к базе данных следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="b8fbb-394">В **имя сервера** введите ваш URL-адрес сервера базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="b8fbb-395">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="b8fbb-396">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="b8fbb-397">Введите имя новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-397">Type a new database name.</span></span>

    <span data-ttu-id="b8fbb-398">![Настройка строки подключения назначения](aspnet-mvc-4-custom-action-filters/_static/image33.png "Настройка целевой строки подключения")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="b8fbb-399">*Настройка целевой строки подключения*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b8fbb-400">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-400">Then click **OK**.</span></span> <span data-ttu-id="b8fbb-401">Когда появится запрос на создание базы данных щелкните **Да**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b8fbb-402">![Создание базы данных](aspnet-mvc-4-custom-action-filters/_static/image34.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="b8fbb-403">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-403">*Creating the database*</span></span>
7. <span data-ttu-id="b8fbb-404">Строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure отображается внутри текстового поля по умолчанию соединения.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b8fbb-405">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-405">Then click **Next**.</span></span>

    <span data-ttu-id="b8fbb-406">![Строка подключения, указывающий базу данных SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "строку соединения, указывающий базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b8fbb-407">*Строка подключения, указывающий базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b8fbb-408">В **предварительного просмотра** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b8fbb-409">![Публикация веб-приложения](aspnet-mvc-4-custom-action-filters/_static/image36.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="b8fbb-410">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="b8fbb-411">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-узла.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="b8fbb-412">Приложение в. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="b8fbb-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="b8fbb-413">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b8fbb-414">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b8fbb-415">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-custom-action-filters/_static/image37.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b8fbb-416">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b8fbb-417">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="b8fbb-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b8fbb-418">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b8fbb-419">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b8fbb-420">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b8fbb-421">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="b8fbb-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b8fbb-422">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b8fbb-423">![Начните вводить имя фрагмента](aspnet-mvc-4-custom-action-filters/_static/image38.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b8fbb-424">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="b8fbb-425">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-custom-action-filters/_static/image39.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b8fbb-426">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b8fbb-427">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-custom-action-filters/_static/image40.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b8fbb-428">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b8fbb-429">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b8fbb-430">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b8fbb-431">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b8fbb-432">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="b8fbb-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b8fbb-433">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-custom-action-filters/_static/image41.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b8fbb-434">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b8fbb-435">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-custom-action-filters/_static/image42.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="b8fbb-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b8fbb-436">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="b8fbb-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
