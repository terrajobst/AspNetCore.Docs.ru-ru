---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 настраиваемых фильтров действий | Документация Майкрософт
author: rick-anderson
description: ASP.NET MVC предоставляет фильтры действий для выполнения логики фильтрации, до или после вызова метода действия. Фильтры действий представляют собой настраиваемые атрибуты tha...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: c435fef624d526ceb01dbc370c5df52e2a1e8350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807939"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="5363d-104">Фильтры настраиваемых действий в ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="5363d-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="5363d-105">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5363d-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5363d-106">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="5363d-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="5363d-107">ASP.NET MVC предоставляет фильтры действий для выполнения логики фильтрации, до или после вызова метода действия.</span><span class="sxs-lookup"><span data-stu-id="5363d-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="5363d-108">Фильтры действий представляют собой настраиваемые атрибуты, которые предоставляют декларативный способ для добавления поведения перед действием и после действия к методам действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="5363d-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="5363d-109">В этой лаборатории вы создадите атрибут фильтра настраиваемое действие в решение MvcMusicStore для перехвата запросов контроллера и записи в журнал действия узла в таблицу базы данных.</span><span class="sxs-lookup"><span data-stu-id="5363d-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="5363d-110">Можно добавить фильтр ведения журнала с помощью внедрения любого контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="5363d-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="5363d-111">Наконец вы увидите представление журнала, в которой отображается список посетителей.</span><span class="sxs-lookup"><span data-stu-id="5363d-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="5363d-112">Это Практическое лабораторное занятие предполагается, что у вас есть базовые знания о **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="5363d-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="5363d-113">Если вы не использовали **ASP.NET MVC** раньше, мы рекомендуем к превышению **ASP.NET MVC 4 Fundamentals** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="5363d-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="5363d-114">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных из в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="5363d-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="5363d-115">Проект, относящиеся к этой лаборатории доступен в [действие настраиваемые фильтры для ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="5363d-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5363d-116">Цели</span><span class="sxs-lookup"><span data-stu-id="5363d-116">Objectives</span></span>

<span data-ttu-id="5363d-117">В этом Практическое лабораторное занятие, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="5363d-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="5363d-118">Создайте атрибут фильтра настраиваемого действия для расширения возможностей фильтрации</span><span class="sxs-lookup"><span data-stu-id="5363d-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="5363d-119">Применение пользовательского атрибута фильтра с помощью внедрения до определенного уровня</span><span class="sxs-lookup"><span data-stu-id="5363d-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="5363d-120">Глобально зарегистрировать настраиваемых фильтров действий</span><span class="sxs-lookup"><span data-stu-id="5363d-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5363d-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5363d-121">Prerequisites</span></span>

<span data-ttu-id="5363d-122">Необходимо иметь следующие элементы во укомплектовать данную лабораторию:</span><span class="sxs-lookup"><span data-stu-id="5363d-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5363d-123">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="5363d-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5363d-124">Установка</span><span class="sxs-lookup"><span data-stu-id="5363d-124">Setup</span></span>

<span data-ttu-id="5363d-125">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="5363d-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="5363d-126">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5363d-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5363d-127">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="5363d-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5363d-128">Если вы не знакомы с фрагменты кода Visual Studio и хотите научиться использовать их, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5363d-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5363d-129">Упражнения</span><span class="sxs-lookup"><span data-stu-id="5363d-129">Exercises</span></span>

<span data-ttu-id="5363d-130">Это Практическое лабораторное занятие включает по следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="5363d-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="5363d-131">Упражнение 1: Ведение журнала действий</span><span class="sxs-lookup"><span data-stu-id="5363d-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="5363d-132">Упражнение 2: Управление нескольких фильтров</span><span class="sxs-lookup"><span data-stu-id="5363d-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="5363d-133">Предполагаемое время для выполнения этого лабораторного: **30 минут**.</span><span class="sxs-lookup"><span data-stu-id="5363d-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="5363d-134">Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="5363d-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="5363d-135">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.</span><span class="sxs-lookup"><span data-stu-id="5363d-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="5363d-136">Упражнение 1: Ведение журнала действий</span><span class="sxs-lookup"><span data-stu-id="5363d-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="5363d-137">В этом упражнении вы узнаете, как создать настраиваемого журнала фильтра действий с помощью поставщиков фильтров ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5363d-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="5363d-138">Для этой цели будет применен фильтр ведения журнала к сайту MusicStore, который будет записывать все действия в выбранных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="5363d-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="5363d-139">Расширить фильтр **ActionFilterAttributeClass** и переопределить **OnActionExecuting** метод перехват каждого запроса, а затем выполнять ведение журнала действий.</span><span class="sxs-lookup"><span data-stu-id="5363d-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="5363d-140">Контекстные сведения об HTTP-запросы, выполнение методов, результаты и параметров будет предоставляться по ASP.NET MVC **ActionExecutingContext** класс **.**</span><span class="sxs-lookup"><span data-stu-id="5363d-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="5363d-141">ASP.NET MVC 4 также имеет поставщиков фильтров по умолчанию, которые можно использовать без создания пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="5363d-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="5363d-142">ASP.NET MVC 4 предоставляет следующие типы фильтров:</span><span class="sxs-lookup"><span data-stu-id="5363d-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="5363d-143">**Авторизация** фильтрации, который принимает решения безопасности, необходимость выполнения метода действия, такие как выполнение проверки подлинности или проверки свойств запроса.</span><span class="sxs-lookup"><span data-stu-id="5363d-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="5363d-144">**Действие** фильтр, который служит оболочкой для выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="5363d-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="5363d-145">Этот фильтр может выполнять дополнительную обработку, например предоставлять дополнительные данные методу действий, инспектировать возвращаемое значение или отменять выполнение метода действия</span><span class="sxs-lookup"><span data-stu-id="5363d-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="5363d-146">**Результат** фильтра, который служит оболочкой для выполнения объекта ActionResult.</span><span class="sxs-lookup"><span data-stu-id="5363d-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="5363d-147">Этот фильтр может выполнять дополнительную обработку результата, например изменить HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="5363d-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="5363d-148">**Исключение** фильтр, который выполняется при наличии необработанного исключения в методе действия, начиная с фильтрами авторизации и заканчивая выполнения результата.</span><span class="sxs-lookup"><span data-stu-id="5363d-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="5363d-149">Фильтры исключений можно использовать для ведения журнала или отображения страниц об ошибках.</span><span class="sxs-lookup"><span data-stu-id="5363d-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="5363d-150">Дополнительные сведения о поставщиках фильтров см. на этой ссылке MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="5363d-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="5363d-151">О возможности ведения журнала в MVC-приложении Music Store</span><span class="sxs-lookup"><span data-stu-id="5363d-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="5363d-152">Это решение Music Store имеет новую таблицу модели данных для ведения журнала узла, **ActionLog**, со следующими полями: имя контроллера, получивший запрос, вызывается действие, IP-адрес клиента и отметку времени.</span><span class="sxs-lookup"><span data-stu-id="5363d-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="5363d-153">![Модель данных. Таблица ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Модели данных. Таблица ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="5363d-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="5363d-154">*Модель данных — ActionLog таблицы*</span><span class="sxs-lookup"><span data-stu-id="5363d-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="5363d-155">Решение обеспечивает представление MVC ASP.NET в журнал действий, который можно найти в **MvcMusicStores/представления/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="5363d-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="5363d-156">![Представления журнала действий](aspnet-mvc-4-custom-action-filters/_static/image2.png "представление журнала действий")</span><span class="sxs-lookup"><span data-stu-id="5363d-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="5363d-157">*Представления журнала действий*</span><span class="sxs-lookup"><span data-stu-id="5363d-157">*Action Log view*</span></span>

<span data-ttu-id="5363d-158">Это структура вся работа будет фокусироваться на прерывание запроса контроллера и выполнять запись в журнал, используя специальную фильтрацию.</span><span class="sxs-lookup"><span data-stu-id="5363d-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="5363d-159">Задача 1 - Создание пользовательского фильтра для перехвата запроса контроллера</span><span class="sxs-lookup"><span data-stu-id="5363d-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="5363d-160">В этой задаче вы создадите класс атрибут пользовательского фильтра, который будет содержать логику занесения данных.</span><span class="sxs-lookup"><span data-stu-id="5363d-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="5363d-161">Для этой цели вы расширите возможности ASP.NET MVC **ActionFilterAttribute** класса и реализовать этот интерфейс **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="5363d-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="5363d-162">**ActionFilterAttribute** является базовым классом для всех атрибутов фильтров.</span><span class="sxs-lookup"><span data-stu-id="5363d-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="5363d-163">Он предоставляет следующие методы для выполнения определенной логики после и перед выполнением действия контроллера:</span><span class="sxs-lookup"><span data-stu-id="5363d-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="5363d-164">**OnActionExecuting**(ActionExecutingContext filterContext): непосредственно перед действие вызывается метод.</span><span class="sxs-lookup"><span data-stu-id="5363d-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="5363d-165">**OnActionExecuted**(ActionExecutedContext filterContext): после вызова метода действия и до выполнения результата (до отображения представления).</span><span class="sxs-lookup"><span data-stu-id="5363d-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="5363d-166">**OnResultExecuting**(ResultExecutingContext filterContext): только в том случае, результат которого выполняется (до отображения представления).</span><span class="sxs-lookup"><span data-stu-id="5363d-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="5363d-167">**OnResultExecuted**(ResultExecutedContext filterContext): после выполнения результата (после визуализации представления).</span><span class="sxs-lookup"><span data-stu-id="5363d-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="5363d-168">Путем переопределения любого из этих методов в производном классе, можно выполнить собственный код фильтрации.</span><span class="sxs-lookup"><span data-stu-id="5363d-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="5363d-169">Откройте **начать** решений, расположенный **\Source\Ex01-LoggingActions\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="5363d-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="5363d-170">Необходимо будет загрузить некоторые отсутствующие пакеты NuGet, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="5363d-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="5363d-171">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5363d-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="5363d-172">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="5363d-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="5363d-173">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="5363d-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5363d-174">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="5363d-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5363d-175">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="5363d-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5363d-176">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="5363d-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="5363d-177">Дополнительные сведения см. в этой статье: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="5363d-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="5363d-178">Добавление нового класса C# в **фильтры** папки и назовите его *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="5363d-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="5363d-179">Эта папка будет хранить все настраиваемые фильтры.</span><span class="sxs-lookup"><span data-stu-id="5363d-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="5363d-180">Откройте **CustomActionFilter.cs** и добавьте ссылку на **System.Web.Mvc** и **MvcMusicStore.Models** пространств имен:</span><span class="sxs-lookup"><span data-stu-id="5363d-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="5363d-181">(Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - сервера Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="5363d-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="5363d-182">Наследовать **CustomActionFilter** класса из **ActionFilterAttribute** и внесите **CustomActionFilter** Реализуйте класс **IActionFilter** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="5363d-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="5363d-183">Сделать **CustomActionFilter** класс переопределить метод **OnActionExecuting** и добавить необходимую логику в журнал выполнения фильтра.</span><span class="sxs-lookup"><span data-stu-id="5363d-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="5363d-184">Чтобы сделать это, добавьте следующий выделенный код в **CustomActionFilter** класса.</span><span class="sxs-lookup"><span data-stu-id="5363d-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="5363d-185">(Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - сервера Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="5363d-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="5363d-186">**OnActionExecuting** использует метод **Entity Framework** для добавления нового ActionLog регистра.</span><span class="sxs-lookup"><span data-stu-id="5363d-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="5363d-187">Он создает и заполняет контекстную информацию из нового экземпляра сущности **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="5363d-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="5363d-188">Дополнительные сведения о **параметром ControllerContext** класса в [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="5363d-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="5363d-189">Задача 2 - Включение перехватчик код в класс контроллера Store</span><span class="sxs-lookup"><span data-stu-id="5363d-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="5363d-190">В этой задаче вы добавите пользовательский фильтр, внедряя его для всех классов контроллеров и действий контроллера, которые регистрируются.</span><span class="sxs-lookup"><span data-stu-id="5363d-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="5363d-191">В целях практики класс контроллера Store будет иметь журнала.</span><span class="sxs-lookup"><span data-stu-id="5363d-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="5363d-192">Метод **OnActionExecuting** из **ActionLogFilterAttribute** пользовательского фильтра выполняется при вызове вставлен элемент.</span><span class="sxs-lookup"><span data-stu-id="5363d-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="5363d-193">Можно также перехватывать метод конкретному контроллеру.</span><span class="sxs-lookup"><span data-stu-id="5363d-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="5363d-194">Откройте **StoreController** в **MvcMusicStore\Controllers** и добавьте ссылку на **фильтры** пространство имен:</span><span class="sxs-lookup"><span data-stu-id="5363d-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="5363d-195">Внедрить пользовательский фильтр **CustomActionFilter** в **StoreController** класса путем добавления **[CustomActionFilter]** атрибут перед объявлением класса.</span><span class="sxs-lookup"><span data-stu-id="5363d-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="5363d-196">Когда фильтр внедряется в класс контроллера, также добавляются все его действия.</span><span class="sxs-lookup"><span data-stu-id="5363d-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="5363d-197">Если вы хотите применить фильтр только для набора действий, пришлось бы внедрить **[CustomActionFilter]** для каждой из них:</span><span class="sxs-lookup"><span data-stu-id="5363d-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5363d-198">Задача 3 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5363d-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="5363d-199">В этой задаче вы проверите, что работает фильтр записи в журнал.</span><span class="sxs-lookup"><span data-stu-id="5363d-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="5363d-200">Вы запустите приложение и посетить магазин, и затем производится проверка журнала действий.</span><span class="sxs-lookup"><span data-stu-id="5363d-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="5363d-201">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="5363d-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5363d-202">Перейдите к **/ActionLog** в начальное состояние представления журнала см. в разделе:</span><span class="sxs-lookup"><span data-stu-id="5363d-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="5363d-203">![Состояние регистрации перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image3.png "tracker состояние перед действием страницы журнала")</span><span class="sxs-lookup"><span data-stu-id="5363d-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="5363d-204">*Состояние регистрации журнала до действия "страницы"*</span><span class="sxs-lookup"><span data-stu-id="5363d-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="5363d-205">По умолчанию он всегда будет отображаться один элемент, который создается при получении существующих жанров для меню.</span><span class="sxs-lookup"><span data-stu-id="5363d-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="5363d-206">В целях упрощения мы удаляем **ActionLog** таблицы при каждом запуске приложения, он отображает только журналы проверки каждой конкретной задачи.</span><span class="sxs-lookup"><span data-stu-id="5363d-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="5363d-207">Может потребоваться удалить следующий код из **сеанса\_запустить** метод (в **Global.asax** класс), чтобы сохранить журнал для всех действий, выполненных в Store Контроллер.</span><span class="sxs-lookup"><span data-stu-id="5363d-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="5363d-208">Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="5363d-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="5363d-209">Перейдите к **/ActionLog** и если журнал пустой press **F5** для обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="5363d-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="5363d-210">Проверьте, что были отслежены посещенных:</span><span class="sxs-lookup"><span data-stu-id="5363d-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="5363d-211">![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image4.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="5363d-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="5363d-212">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="5363d-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="5363d-213">Упражнение 2: Управление нескольких фильтров</span><span class="sxs-lookup"><span data-stu-id="5363d-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="5363d-214">В этом упражнении будет добавить второй настраиваемый фильтр действий в класс StoreController и определить определенном порядке, в котором будет выполняться оба фильтра.</span><span class="sxs-lookup"><span data-stu-id="5363d-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="5363d-215">Затем вы обновите код, чтобы зарегистрировать глобальный фильтр.</span><span class="sxs-lookup"><span data-stu-id="5363d-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="5363d-216">Существуют различные параметры, которые необходимо учитывать при определении порядка выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="5363d-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="5363d-217">Например свойство Order и область фильтры:</span><span class="sxs-lookup"><span data-stu-id="5363d-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="5363d-218">Можно определить **область** для каждого из фильтров, например, можно задать область все фильтры действий для выполнения в рамках **область контроллера**и все фильтры авторизации для запуска в **глобальной области видимости** .</span><span class="sxs-lookup"><span data-stu-id="5363d-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="5363d-219">Области имеют порядок определения выполнение.</span><span class="sxs-lookup"><span data-stu-id="5363d-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="5363d-220">Кроме того для каждого фильтра действия есть свойство Order, который используется для определения порядка выполнения в области фильтра.</span><span class="sxs-lookup"><span data-stu-id="5363d-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="5363d-221">Дополнительные сведения о порядке выполнения пользовательских фильтров действий см. на этой статье MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="5363d-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="5363d-222">Упражнение 1: Создание нового настраиваемого фильтра действий</span><span class="sxs-lookup"><span data-stu-id="5363d-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="5363d-223">В этой задаче вы создадите новый настраиваемый фильтр действий для добавления в класс StoreController научиться управлять порядок выполнения фильтров.</span><span class="sxs-lookup"><span data-stu-id="5363d-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="5363d-224">Откройте **начать** решений, расположенный **\Source\Ex02-ManagingMultipleActionFilters\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="5363d-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="5363d-225">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="5363d-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5363d-226">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="5363d-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5363d-227">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5363d-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5363d-228">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="5363d-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5363d-229">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="5363d-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="5363d-230">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="5363d-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5363d-231">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="5363d-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5363d-232">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="5363d-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="5363d-233">Дополнительные сведения см. в этой статье: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="5363d-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="5363d-234">Добавление нового класса C# в **фильтры** папки и назовите его *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="5363d-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="5363d-235">Откройте **MyNewCustomActionFilter.cs** и добавьте ссылку на **System.Web.Mvc** и **MvcMusicStore.Models** пространство имен:</span><span class="sxs-lookup"><span data-stu-id="5363d-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="5363d-236">(Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="5363d-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="5363d-237">Замените объявление класса по умолчанию следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="5363d-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="5363d-238">(Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="5363d-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="5363d-239">Этот настраиваемый фильтр действий практически не отличается от той, которую вы создали в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="5363d-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="5363d-240">Основное различие заключается в наличии *&quot;войти по&quot;* атрибута обновляется с именем этого нового класса, чтобы определить фильтр, который зарегистрирован в журнале.</span><span class="sxs-lookup"><span data-stu-id="5363d-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="5363d-241">Задача 2: Включение новый перехватчик код в класс StoreController</span><span class="sxs-lookup"><span data-stu-id="5363d-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="5363d-242">В этой задаче будет добавить новый пользовательский фильтр в класс StoreController и запустить решение, чтобы убедиться в том, как оба фильтра совместной работы.</span><span class="sxs-lookup"><span data-stu-id="5363d-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="5363d-243">Откройте **StoreController** класса, расположенный **MvcMusicStore\Controllers** и внедрять новый настраиваемый фильтр **MyNewCustomActionFilter** в  **StoreController** показан класс, как в следующем коде.</span><span class="sxs-lookup"><span data-stu-id="5363d-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="5363d-244">Теперь запустите приложение, чтобы показать, как работают эти две настраиваемые фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="5363d-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="5363d-245">Чтобы сделать это, нажмите клавишу **F5** и подождите, пока при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="5363d-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="5363d-246">Перейдите к **/ActionLog** в начальное состояние представления журнала см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="5363d-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="5363d-247">![Состояние регистрации перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image5.png "tracker состояние перед действием страницы журнала")</span><span class="sxs-lookup"><span data-stu-id="5363d-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="5363d-248">*Состояние регистрации журнала до действия "страницы"*</span><span class="sxs-lookup"><span data-stu-id="5363d-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="5363d-249">Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="5363d-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="5363d-250">Убедитесь, что это время; посещенных отслеживались дважды: после добавления в пользовательские фильтры действий во всех **StorageController** класса.</span><span class="sxs-lookup"><span data-stu-id="5363d-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="5363d-251">![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image6.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="5363d-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="5363d-252">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="5363d-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="5363d-253">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="5363d-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="5363d-254">Задача 3: Управление упорядочение фильтра</span><span class="sxs-lookup"><span data-stu-id="5363d-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="5363d-255">В этой задаче вы узнаете, как управлять порядок выполнения фильтров с помощью свойства заказа.</span><span class="sxs-lookup"><span data-stu-id="5363d-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="5363d-256">Откройте **StoreController** класса, расположенный **MvcMusicStore\Controllers** и укажите **порядок** свойство в обоих фильтров, такие как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="5363d-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="5363d-257">Убедитесь том, как фильтры выполняются в зависимости от значения свойства Order.</span><span class="sxs-lookup"><span data-stu-id="5363d-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="5363d-258">Вы увидите, что фильтр с наименьшим значением порядка (**CustomActionFilter**) является тот, который выполняется.</span><span class="sxs-lookup"><span data-stu-id="5363d-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="5363d-259">Нажмите клавишу **F5** и подождите, пока при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="5363d-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="5363d-260">Перейдите к **/ActionLog** в начальное состояние представления журнала см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="5363d-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="5363d-261">![Состояние регистрации перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image7.png "tracker состояние перед действием страницы журнала")</span><span class="sxs-lookup"><span data-stu-id="5363d-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="5363d-262">*Состояние регистрации журнала до действия "страницы"*</span><span class="sxs-lookup"><span data-stu-id="5363d-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="5363d-263">Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="5363d-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="5363d-264">Проверьте, что на этот раз посещенных отслеживались упорядоченные по фильтрации порядковый номер: **CustomActionFilter** журналы первого.</span><span class="sxs-lookup"><span data-stu-id="5363d-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="5363d-265">![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image8.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="5363d-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="5363d-266">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="5363d-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="5363d-267">Теперь обновления значение порядка фильтров и проверьте, как изменяется порядок ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="5363d-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="5363d-268">В **StoreController** класса, обновите значение порядка фильтров, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="5363d-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="5363d-269">Снова запустите приложение, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="5363d-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="5363d-270">Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="5363d-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="5363d-271">Проверьте, что на этот раз журналов, созданных **MyNewCustomActionFilter** фильтра отображается первым.</span><span class="sxs-lookup"><span data-stu-id="5363d-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="5363d-272">![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image9.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="5363d-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="5363d-273">*Журнал действий с действием в журнал*</span><span class="sxs-lookup"><span data-stu-id="5363d-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="5363d-274">Задача 4: Регистрация фильтрует глобально</span><span class="sxs-lookup"><span data-stu-id="5363d-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="5363d-275">В этой задаче вы обновите решение, чтобы зарегистрировать новый фильтр (**MyNewCustomActionFilter**) как глобальный фильтр.</span><span class="sxs-lookup"><span data-stu-id="5363d-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="5363d-276">Таким образом, он будет запускаться все perfomed действия в приложении, а не только StoreController из них как в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="5363d-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="5363d-277">В **StoreController** класса, удалите **[MyNewCustomActionFilter]** атрибутов и свойств заказа из **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="5363d-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="5363d-278">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5363d-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="5363d-279">Откройте **Global.asax** файл и найдите **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="5363d-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="5363d-280">Обратите внимание на то, что при каждом запуске приложения оно регистрация глобальных фильтров путем вызова **RegisterGlobalFilters** метода в **FilterConfig** класса.</span><span class="sxs-lookup"><span data-stu-id="5363d-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="5363d-281">![Регистрация глобальных фильтров в файле Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "регистрация глобальных фильтров в файле Global.asax")</span><span class="sxs-lookup"><span data-stu-id="5363d-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="5363d-282">*Регистрация глобальных фильтров в файле Global.asax*</span><span class="sxs-lookup"><span data-stu-id="5363d-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="5363d-283">Откройте **FilterConfig.cs** файл включен в **приложения\_запустить** папки.</span><span class="sxs-lookup"><span data-stu-id="5363d-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="5363d-284">Добавьте ссылку на использование System.Web.Mvc; с помощью MvcMusicStore.Filters; пространство имен.</span><span class="sxs-lookup"><span data-stu-id="5363d-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="5363d-285">Обновление **RegisterGlobalFilters** метод добавления пользовательского фильтра.</span><span class="sxs-lookup"><span data-stu-id="5363d-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="5363d-286">Чтобы сделать это, добавьте выделенный код:</span><span class="sxs-lookup"><span data-stu-id="5363d-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="5363d-287">Запустите приложение, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="5363d-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="5363d-288">Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.</span><span class="sxs-lookup"><span data-stu-id="5363d-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="5363d-289">Проверьте, что теперь **[MyNewCustomActionFilter]** является, введенного в HomeController и ActionLogController слишком.</span><span class="sxs-lookup"><span data-stu-id="5363d-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="5363d-290">![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image11.png "журнал действий с действием в журнал")</span><span class="sxs-lookup"><span data-stu-id="5363d-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="5363d-291">*Журнал действий с помощью глобального действия в журнал*</span><span class="sxs-lookup"><span data-stu-id="5363d-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="5363d-292">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложении B: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="5363d-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5363d-293">Сводка</span><span class="sxs-lookup"><span data-stu-id="5363d-293">Summary</span></span>

<span data-ttu-id="5363d-294">Выполнив этот практический семинар вы узнали, как расширить фильтр действий для выполнения пользовательских действий.</span><span class="sxs-lookup"><span data-stu-id="5363d-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="5363d-295">Вы также узнали, как внедрить любой фильтр к контроллерам страницы.</span><span class="sxs-lookup"><span data-stu-id="5363d-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="5363d-296">Использовались следующие понятия:</span><span class="sxs-lookup"><span data-stu-id="5363d-296">The following concepts were used:</span></span>

- <span data-ttu-id="5363d-297">Создание фильтров настраиваемое действие в классе ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="5363d-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="5363d-298">Как внедрить фильтры в контроллерах ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5363d-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="5363d-299">Как управлять фильтра упорядочение с помощью свойства Order</span><span class="sxs-lookup"><span data-stu-id="5363d-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="5363d-300">Как зарегистрировать фильтры глобально</span><span class="sxs-lookup"><span data-stu-id="5363d-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5363d-301">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="5363d-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5363d-302">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="5363d-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5363d-303">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="5363d-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5363d-304">Перейдите к [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5363d-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5363d-305">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="5363d-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="5363d-306">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="5363d-306">Click on **Install Now**.</span></span> <span data-ttu-id="5363d-307">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="5363d-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5363d-308">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="5363d-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5363d-309">![Установка Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5363d-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5363d-310">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5363d-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5363d-311">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="5363d-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="5363d-313">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="5363d-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5363d-314">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="5363d-314">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="5363d-316">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="5363d-316">*Installation progress*</span></span>
6. <span data-ttu-id="5363d-317">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="5363d-317">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="5363d-319">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="5363d-319">*Installation completed*</span></span>
7. <span data-ttu-id="5363d-320">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="5363d-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5363d-321">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="5363d-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="5363d-323">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="5363d-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="5363d-324">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="5363d-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="5363d-325">В этом приложении показано, как создать новый веб-сайт на портале управления Windows Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="5363d-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="5363d-326">Задача 1 - Создание нового веб-сайта с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5363d-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="5363d-327">Перейдите к [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="5363d-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5363d-328">С помощью Windows Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика.</span><span class="sxs-lookup"><span data-stu-id="5363d-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="5363d-329">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="5363d-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="5363d-330">![Войдите на портал Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Войдите на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="5363d-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="5363d-331">*Войдите на портал управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="5363d-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="5363d-332">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="5363d-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="5363d-333">![Создание нового веб-сайта](aspnet-mvc-4-custom-action-filters/_static/image18.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="5363d-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="5363d-334">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="5363d-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="5363d-335">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="5363d-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="5363d-336">Затем выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="5363d-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="5363d-337">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="5363d-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5363d-338">Веб-сайта Windows Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="5363d-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="5363d-339">Быстрое создание позволяет развернуть завершенное веб-приложения для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="5363d-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="5363d-340">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="5363d-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="5363d-341">![Создание нового веб-сайта, с помощью быстрого создания](aspnet-mvc-4-custom-action-filters/_static/image19.png "создания нового веб-сайта, с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="5363d-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="5363d-342">*Создание нового веб-сайта, с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="5363d-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="5363d-343">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="5363d-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="5363d-344">После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="5363d-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="5363d-345">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="5363d-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="5363d-346">![Выбрав новый веб-сайт](aspnet-mvc-4-custom-action-filters/_static/image20.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="5363d-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="5363d-347">*Выбрав новый веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="5363d-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="5363d-348">![Веб-сайт работал](aspnet-mvc-4-custom-action-filters/_static/image21.png "веб-узлом")</span><span class="sxs-lookup"><span data-stu-id="5363d-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="5363d-349">*Веб-сайт под управлением*</span><span class="sxs-lookup"><span data-stu-id="5363d-349">*Web site running*</span></span>
6. <span data-ttu-id="5363d-350">Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="5363d-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="5363d-351">![Открытие страницы управления веб-сайт](aspnet-mvc-4-custom-action-filters/_static/image22.png "Открытие страницы управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="5363d-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="5363d-352">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="5363d-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="5363d-353">В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="5363d-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5363d-354">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайт Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="5363d-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="5363d-355">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации.</span><span class="sxs-lookup"><span data-stu-id="5363d-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="5363d-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений на веб-сайтах Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="5363d-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="5363d-357">![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-custom-action-filters/_static/image23.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="5363d-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="5363d-358">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="5363d-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="5363d-359">Скачайте файл профиля публикации в известном месте.</span><span class="sxs-lookup"><span data-stu-id="5363d-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="5363d-360">Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения на веб-сайтах, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5363d-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="5363d-361">![Сохранение файла профиля публикации](aspnet-mvc-4-custom-action-filters/_static/image24.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="5363d-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="5363d-362">*Сохранение файла профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="5363d-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="5363d-363">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="5363d-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="5363d-364">Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="5363d-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="5363d-365">Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5363d-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="5363d-366">Вам потребуется сервер базы данных SQL для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="5363d-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="5363d-367">Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="5363d-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="5363d-368">Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="5363d-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="5363d-369">Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="5363d-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="5363d-370">Не создавайте базы данных, так как он будет создан в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="5363d-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="5363d-371">![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-custom-action-filters/_static/image25.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="5363d-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="5363d-372">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="5363d-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="5363d-373">В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="5363d-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="5363d-374">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="5363d-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Добавление IP-адрес клиента](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="5363d-376">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="5363d-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="5363d-377">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="5363d-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="5363d-379">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="5363d-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="5363d-380">Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="5363d-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="5363d-381">Вернитесь к решению для ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5363d-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="5363d-382">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="5363d-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="5363d-383">![Публикация приложения](aspnet-mvc-4-custom-action-filters/_static/image29.png "публикации приложения")</span><span class="sxs-lookup"><span data-stu-id="5363d-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="5363d-384">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="5363d-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="5363d-385">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="5363d-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="5363d-386">![Импорт профиля публикации](aspnet-mvc-4-custom-action-filters/_static/image30.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="5363d-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="5363d-387">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="5363d-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="5363d-388">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="5363d-388">Click **Validate Connection**.</span></span> <span data-ttu-id="5363d-389">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5363d-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5363d-390">Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.</span><span class="sxs-lookup"><span data-stu-id="5363d-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="5363d-391">![Проверка подключения](aspnet-mvc-4-custom-action-filters/_static/image31.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="5363d-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="5363d-392">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="5363d-392">*Validating connection*</span></span>
4. <span data-ttu-id="5363d-393">В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="5363d-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="5363d-394">![Конфигурация веб-развертывания](aspnet-mvc-4-custom-action-filters/_static/image32.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="5363d-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="5363d-395">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="5363d-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="5363d-396">Настройте подключение к базе данных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5363d-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="5363d-397">В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="5363d-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="5363d-398">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="5363d-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="5363d-399">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="5363d-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="5363d-400">Введите новое имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="5363d-400">Type a new database name.</span></span>

     <span data-ttu-id="5363d-401">![Настройка строки подключения назначения](aspnet-mvc-4-custom-action-filters/_static/image33.png "Настройка строка соединения с назначением")</span><span class="sxs-lookup"><span data-stu-id="5363d-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="5363d-402">*Настройка строки подключения назначения*</span><span class="sxs-lookup"><span data-stu-id="5363d-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="5363d-403">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5363d-403">Then click **OK**.</span></span> <span data-ttu-id="5363d-404">При появлении запроса на создание базы данных нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="5363d-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="5363d-405">![Создание базы данных](aspnet-mvc-4-custom-action-filters/_static/image34.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="5363d-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="5363d-406">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="5363d-406">*Creating the database*</span></span>
7. <span data-ttu-id="5363d-407">В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="5363d-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="5363d-408">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5363d-408">Then click **Next**.</span></span>

    <span data-ttu-id="5363d-409">![Строка подключения, указывающий на базу данных SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "строку подключения, указывающий на базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="5363d-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="5363d-410">*Строка подключения, указывающий на базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="5363d-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="5363d-411">В **предварительной версии** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="5363d-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="5363d-412">![Публикация веб-приложения](aspnet-mvc-4-custom-action-filters/_static/image36.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="5363d-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="5363d-413">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="5363d-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="5363d-414">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="5363d-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="5363d-415">Приложение c. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="5363d-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="5363d-416">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="5363d-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5363d-417">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="5363d-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5363d-418">![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-custom-action-filters/_static/image37.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="5363d-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5363d-419">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="5363d-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5363d-420">***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="5363d-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5363d-421">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="5363d-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5363d-422">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="5363d-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5363d-423">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="5363d-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5363d-424">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="5363d-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5363d-425">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="5363d-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5363d-426">![Начните вводить имя фрагмента](aspnet-mvc-4-custom-action-filters/_static/image38.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="5363d-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5363d-427">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="5363d-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="5363d-428">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-custom-action-filters/_static/image39.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="5363d-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5363d-429">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="5363d-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5363d-430">![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-custom-action-filters/_static/image40.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="5363d-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5363d-431">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="5363d-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5363d-432">***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5363d-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5363d-433">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="5363d-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5363d-434">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="5363d-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5363d-435">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="5363d-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5363d-436">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-custom-action-filters/_static/image41.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="5363d-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5363d-437">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="5363d-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5363d-438">![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-custom-action-filters/_static/image42.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="5363d-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5363d-439">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="5363d-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
