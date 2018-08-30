---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Новые возможности в ASP.NET MVC 4 | Документация Майкрософт
author: rick-anderson
description: ASP.NET MVC 4 — это платформа для создания масштабируемых, основанные на стандартах веб-приложений, с помощью хорошо проверенных шаблонах проектирования и возможности ASP.NET и...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: b1d80928d765bc71ea1579272662b6697371c47b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837232"
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="94882-103">Новые возможности в ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="94882-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="94882-104">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="94882-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="94882-105">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="94882-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="94882-106">ASP.NET MVC 4 — это платформа для создания масштабируемых, основанные на стандартах веб-приложений, с помощью хорошо проверенных шаблонах проектирования и возможности ASP.NET и .NET framework.</span><span class="sxs-lookup"><span data-stu-id="94882-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="94882-107">Этот новый, четвертая версия платформы посвящена упрощение разработки мобильных веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="94882-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="94882-108">Начнем с того при создании нового проекта ASP.NET MVC 4 теперь есть шаблон проекта мобильного приложения, которые можно использовать для построения это автономное приложение, в частности для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="94882-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="94882-109">Кроме того ASP.NET MVC 4 интегрируется с jQuery Mobile посредством пакета NuGet jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="94882-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="94882-110">jQuery Mobile — это платформа на базе HTML5, для разработки веб-приложений, совместимых с всех платформ популярных мобильных устройствах, включая Windows Phone, iPhone, Android и т. д.</span><span class="sxs-lookup"><span data-stu-id="94882-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="94882-111">Тем не менее если вам нужна специализации, ASP.NET MVC 4 также позволяет обслуживать различные представления для разных устройств и предоставить проводить оптимизацию для конкретного устройства.</span><span class="sxs-lookup"><span data-stu-id="94882-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="94882-112">В этой практической работу вы начнете с ASP.NET MVC 4 &quot;веб-приложение&quot; шаблон проекта для создания приложения Photo Gallery.</span><span class="sxs-lookup"><span data-stu-id="94882-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="94882-113">Постепенно, поможет повысить эффективность приложения с помощью jQuery Mobile и новые функции ASP.NET MVC 4, чтобы обеспечить его совместимость с различных мобильных устройств и классических веб-браузеров.</span><span class="sxs-lookup"><span data-stu-id="94882-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="94882-114">Вы также узнаете о новых рецепты для создания кода для создания кода и как ASP.NET MVC 4 упрощает написание асинхронных методов действия, поддерживая задач&lt;ActionResult&gt; типы возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="94882-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="94882-115">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="94882-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="94882-116">Проект, относящиеся к этой лаборатории доступен в [новые возможности в веб-форм в ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span><span class="sxs-lookup"><span data-stu-id="94882-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="94882-117">Цели</span><span class="sxs-lookup"><span data-stu-id="94882-117">Objectives</span></span>

<span data-ttu-id="94882-118">В этой практической работу вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="94882-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="94882-119">Воспользоваться преимуществами расширений для ASP.NET MVC проект шаблоны, в том числе нового шаблона проекта мобильного приложения</span><span class="sxs-lookup"><span data-stu-id="94882-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="94882-120">Использовать окна просмотра атрибут HTML5 и CSS медиа-запросов для улучшения отображения на мобильных устройствах</span><span class="sxs-lookup"><span data-stu-id="94882-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="94882-121">Использовать jQuery Mobile для прогрессивного улучшения, а также для создания оптимизированных для сенсорных экранов веб-Интерфейс</span><span class="sxs-lookup"><span data-stu-id="94882-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="94882-122">Создание представлений для мобильных устройств</span><span class="sxs-lookup"><span data-stu-id="94882-122">Create mobile-specific views</span></span>
- <span data-ttu-id="94882-123">Использовать компонент переключателя для переключения между представлениями мобильные и Классические приложения</span><span class="sxs-lookup"><span data-stu-id="94882-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="94882-124">Создание асинхронных контроллеров, используя поддержку задач</span><span class="sxs-lookup"><span data-stu-id="94882-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="94882-125">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="94882-125">Prerequisites</span></span>

<span data-ttu-id="94882-126">Необходимо иметь следующие элементы во укомплектовать данную лабораторию:</span><span class="sxs-lookup"><span data-stu-id="94882-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="94882-127">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложении Б](#AppendixB) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="94882-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="94882-128">[ASP.NET MVC 4](../../../mvc4.md) (входит в установку Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="94882-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="94882-129">Эмулятор Windows Phone (включен в [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="94882-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="94882-130">Необязательно — [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) с **Electric Plum iPhone симулятор** расширения (только для Упражнение 3 используется для просмотра веб-приложения с помощью эмулятор iPhone)</span><span class="sxs-lookup"><span data-stu-id="94882-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="94882-131">Установка</span><span class="sxs-lookup"><span data-stu-id="94882-131">Setup</span></span>

<span data-ttu-id="94882-132">В этом документе лаборатории вам будет рекомендовано вставлять блоки кода.</span><span class="sxs-lookup"><span data-stu-id="94882-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="94882-133">Для удобства большая часть этого кода предоставляется как фрагменты кода Visual Studio, который можно использовать из среды Visual Studio, чтобы избежать необходимости добавлять ее вручную.</span><span class="sxs-lookup"><span data-stu-id="94882-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="94882-134">Чтобы установить фрагменты кода.</span><span class="sxs-lookup"><span data-stu-id="94882-134">To install the code snippets:</span></span>

1. <span data-ttu-id="94882-135">Откройте окно проводника Windows и перейдите к лаборатории **Source\Setup** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="94882-136">Дважды щелкните **Setup.cmd** файл в эту папку, чтобы установить фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="94882-137">Если вы не знакомы с фрагменты кода Visual Studio и хотите научиться использовать их, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложении A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="94882-138">Упражнения</span><span class="sxs-lookup"><span data-stu-id="94882-138">Exercises</span></span>

<span data-ttu-id="94882-139">Это Практическое занятие включает в себя следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="94882-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="94882-140">Новые шаблоны проектов ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="94882-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="94882-141">Создание веб-приложение фотоальбома</span><span class="sxs-lookup"><span data-stu-id="94882-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="94882-142">Добавление поддержки для мобильных устройств</span><span class="sxs-lookup"><span data-stu-id="94882-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="94882-143">Использование асинхронных контроллеров</span><span class="sxs-lookup"><span data-stu-id="94882-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="94882-144">Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="94882-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="94882-145">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.</span><span class="sxs-lookup"><span data-stu-id="94882-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="94882-146">Предполагаемое время для выполнения этого лабораторного: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="94882-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="94882-147">Упражнение 1: Новые шаблоны проектов ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="94882-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="94882-148">В этом упражнении будет Узнайте об усовершенствованиях в шаблонах проекта ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="94882-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="94882-149">Дополнение к шаблону веб-приложение, уже присутствующего в MVC 3, эта версия теперь включает отдельный шаблон для мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="94882-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="94882-150">Во-первых будут рассмотрены некоторые соответствующие функции каждого из шаблонов.</span><span class="sxs-lookup"><span data-stu-id="94882-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="94882-151">Затем будет работать над визуализацией страницы правильно на разных платформах с помощью правильного подхода.</span><span class="sxs-lookup"><span data-stu-id="94882-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="94882-152">Задача 1 - изучение данный шаблон приложения Интернета</span><span class="sxs-lookup"><span data-stu-id="94882-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="94882-153">Откройте **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="94882-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="94882-154">Выберите **файл | Новые | Проект** команды меню.</span><span class="sxs-lookup"><span data-stu-id="94882-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="94882-155">В **новый проект** диалоговом окне выберите **Visual C# | Web** шаблона на левой панели дерева, а затем выберите **веб-приложение ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="94882-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="94882-156">Назовите проект **PhotoGallery**, выберите расположение (или оставьте значение по умолчанию) и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-157">После этого следует настроить решение Коллекция фотографий ASP.NET MVC 4, которое теперь создается.</span><span class="sxs-lookup"><span data-stu-id="94882-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="94882-158">![Создание нового проекта](whats-new-in-aspnet-mvc-4/_static/image1.png "создания нового проекта")</span><span class="sxs-lookup"><span data-stu-id="94882-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="94882-159">*Создание нового проекта*</span><span class="sxs-lookup"><span data-stu-id="94882-159">*Creating a new project*</span></span>
3. <span data-ttu-id="94882-160">В **создания проекта ASP.NET MVC 4** диалоговом окне выберите **веб-приложение** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="94882-161">Убедитесь, что выбран в качестве обработчика представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="94882-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="94882-162">![Создание нового приложения ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "Создание нового приложения ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="94882-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="94882-163">*Создание нового приложения ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="94882-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-164">Синтаксис Razor представлена в ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="94882-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="94882-165">Наша цель — чтобы свести к минимуму число знаков и нажатий клавиш в файл, позволяя быстро и жидкостей, рабочие процессы кодирования.</span><span class="sxs-lookup"><span data-stu-id="94882-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="94882-166">Razor использует существующие C# / VB (или других) языковые навыки и обеспечивает синтаксис разметки шаблона, который позволяет awesome рабочего процесса создания HTML.</span><span class="sxs-lookup"><span data-stu-id="94882-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="94882-167">Нажмите клавишу **F5** для запуска решения, а также видеть обновленные шаблоны.</span><span class="sxs-lookup"><span data-stu-id="94882-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="94882-168">Вы можете проверить следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="94882-168">You can check out the following features:</span></span>

    <span data-ttu-id="94882-169">**Шаблоны в современном стиле**</span><span class="sxs-lookup"><span data-stu-id="94882-169">**Modern-style templates**</span></span>

    <span data-ttu-id="94882-170">Шаблоны были обновлены, предоставляя дополнительные стили современных.</span><span class="sxs-lookup"><span data-stu-id="94882-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="94882-171">![Шаблоны ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "restyled шаблонов MVC 4")</span><span class="sxs-lookup"><span data-stu-id="94882-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="94882-172">*Шаблоны ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="94882-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="94882-173">![Новая страница контакта](whats-new-in-aspnet-mvc-4/_static/image4.png "страницы нового контакта")</span><span class="sxs-lookup"><span data-stu-id="94882-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="94882-174">*Новая страница контакта*</span><span class="sxs-lookup"><span data-stu-id="94882-174">*New Contact page*</span></span>

    <span data-ttu-id="94882-175">**Адаптивная отрисовка**</span><span class="sxs-lookup"><span data-stu-id="94882-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="94882-176">Извлеките изменения размера окна браузера и обратите внимание на то, как макет страницы приспосабливается к изменениям для нового размера окна.</span><span class="sxs-lookup"><span data-stu-id="94882-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="94882-177">Эти шаблоны использовать методика адаптивной отрисовки для неправильно отображается на настольных и мобильных платформах без какой-либо настройки.</span><span class="sxs-lookup"><span data-stu-id="94882-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="94882-178">![Шаблон проекта ASP.NET MVC 4 в другом браузере размеры](whats-new-in-aspnet-mvc-4/_static/image5.png "шаблон проекта ASP.NET MVC 4 в другом браузере размеры")</span><span class="sxs-lookup"><span data-stu-id="94882-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="94882-179">*Шаблон проекта ASP.NET MVC 4 в другом браузере размеры*</span><span class="sxs-lookup"><span data-stu-id="94882-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="94882-180">**Более широкие пользовательского интерфейса с помощью JavaScript**</span><span class="sxs-lookup"><span data-stu-id="94882-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="94882-181">Другое усовершенствование шаблоны проекта по умолчанию является использование JavaScript для предоставления более интерактивной JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94882-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="94882-182">Вход и регистрация ссылки, используемые в шаблоне проиллюстрировать, как с помощью проверки jQuery проверять поля ввода со стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="94882-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery Validation](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="94882-184">*jQuery Validation*</span><span class="sxs-lookup"><span data-stu-id="94882-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-185">Обратите внимание, что два журнала в разделах, в первом разделе можно войдите в систему под учетной записью зарегистрированный на сайте и во втором разделе, вы можете altenativelly вход с использованием другую службу проверки подлинности, например google (отключено по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="94882-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="94882-186">Закройте браузер, чтобы остановить отладчик и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="94882-187">Откройте файл **AuthConfig.cs** каталоге **приложения\_запустить** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="94882-188">Удалите комментарий в последней строке регистрацию клиента Google для *OAuth* проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="94882-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="94882-189">Обратите внимание, что вы можете легко включить проверку подлинности с помощью любой службы OpenID или OAuth, таких как Facebook, Twitter, Microsoft, и т.д.</span><span class="sxs-lookup"><span data-stu-id="94882-189">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="94882-190">Нажмите клавишу **F5** для запуска решения, а затем перейдите на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="94882-190">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="94882-191">Выберите **Google** службу для входа.</span><span class="sxs-lookup"><span data-stu-id="94882-191">Select **Google** service to log in.</span></span>

    ![Выбор журнала в службе](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="94882-193">*Выбор журнала в службе*</span><span class="sxs-lookup"><span data-stu-id="94882-193">*Selecting the log in service*</span></span>
10. <span data-ttu-id="94882-194">Вход с помощью учетной записи Google.</span><span class="sxs-lookup"><span data-stu-id="94882-194">Log in using your Google account.</span></span>
11. <span data-ttu-id="94882-195">Разрешить переход на сайт (localhost) для получения сведений из учетной записи Google.</span><span class="sxs-lookup"><span data-stu-id="94882-195">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="94882-196">Наконец вам будет необходимо зарегистрировать на сайте, чтобы связать учетную запись Google.</span><span class="sxs-lookup"><span data-stu-id="94882-196">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![Связать учетную запись Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="94882-198">*Связывание учетной записи Google*</span><span class="sxs-lookup"><span data-stu-id="94882-198">*Associating your Google account*</span></span>
13. <span data-ttu-id="94882-199">Закройте браузер, чтобы остановить отладчик и вернитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-199">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="94882-200">Теперь ознакомьтесь с решением для извлечения некоторые новые возможности, появившиеся в ASP.NET MVC 4 в шаблоне проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-200">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="94882-201">![Шаблон проекта приложения ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "шаблон проекта приложения ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="94882-201">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="94882-202">*Шаблон проекта приложения ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="94882-202">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="94882-203">**HTML 5 разметки**</span><span class="sxs-lookup"><span data-stu-id="94882-203">**HTML 5 Markup**</span></span>

       <span data-ttu-id="94882-204">Обзор представлений шаблона, чтобы узнать, Новая разметка темы.</span><span class="sxs-lookup"><span data-stu-id="94882-204">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="94882-205">![Новый шаблон с помощью Razor и HTML5 About.cshtml разметки. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Новый шаблон, с помощью Razor и HTML5 About.cshtml разметки.")</span><span class="sxs-lookup"><span data-stu-id="94882-205">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="94882-206">*Новый шаблон, с помощью разметки Razor и HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="94882-206">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="94882-207">**Обновленные библиотеки JavaScript**</span><span class="sxs-lookup"><span data-stu-id="94882-207">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="94882-208">Шаблон по умолчанию ASP.NET MVC 4 теперь включает KnockoutJS, платформу JavaScript MVVM, которая позволяет создавать разнообразные и малым временем отклика веб-приложений, с помощью JavaScript и HTML.</span><span class="sxs-lookup"><span data-stu-id="94882-208">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="94882-209">Как и в MVC3, jQuery и библиотеки пользовательского интерфейса jQuery также включаются в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="94882-209">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="94882-210">Можно получить дополнительные сведения о библиотеке KnockOutJS в этой связи: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="94882-210">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="94882-211">Кроме того, можно узнать о jQuery и пользовательский Интерфейс jQuery в [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="94882-211">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="94882-212">Задача 2 - изучение шаблона мобильного приложения</span><span class="sxs-lookup"><span data-stu-id="94882-212">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="94882-213">ASP.NET MVC 4 облегчает разработку веб-сайтов для мобильных устройств и браузеров планшета.</span><span class="sxs-lookup"><span data-stu-id="94882-213">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="94882-214">Этот шаблон имеет такую же структуру приложения, как шаблон веб-приложение (Обратите внимание, что код контроллера реализована практически идентично), но его стиль был изменен для неправильно отображается на сенсорным управлением мобильными устройствами.</span><span class="sxs-lookup"><span data-stu-id="94882-214">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="94882-215">Выберите **файл | Новые | Проект** команды меню.</span><span class="sxs-lookup"><span data-stu-id="94882-215">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="94882-216">В **новый проект** диалоговом окне выберите **Visual C# | Web** шаблона на левой панели дерева, а затем выберите **веб-приложение ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="94882-216">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="94882-217">Назовите проект **PhotoGallery.Mobile**, выберите расположение (или оставьте значение по умолчанию), выберите &quot;добавить решение&quot; и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-217">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="94882-218">В **создания проекта ASP.NET MVC 4** диалоговом окне выберите **мобильного приложения** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-218">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="94882-219">Убедитесь, что выбран в качестве обработчика представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="94882-219">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="94882-220">![Создание нового приложения ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "Создание нового приложения ASP.NET MVC 4 Mobile")</span><span class="sxs-lookup"><span data-stu-id="94882-220">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="94882-221">*Создание нового приложения ASP.NET MVC 4 Mobile*</span><span class="sxs-lookup"><span data-stu-id="94882-221">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="94882-222">Теперь вы сможете просматривать решения и некоторые новые функции, представленные в шаблоне решения ASP.NET MVC 4 для мобильных устройств:</span><span class="sxs-lookup"><span data-stu-id="94882-222">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="94882-223">**jQuery Mobile библиотеки**</span><span class="sxs-lookup"><span data-stu-id="94882-223">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="94882-224">Шаблон проекта мобильного приложения включает в себя мобильных библиотеку jQuery, которая является библиотекой с открытым исходным кодом для совместимости браузера мобильного устройства.</span><span class="sxs-lookup"><span data-stu-id="94882-224">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="94882-225">jQuery Mobile применяется прогрессивное Совершенствование для браузеров мобильных устройств, которые поддерживают CSS и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94882-225">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="94882-226">Прогрессивное Совершенствование включает все обозреватели для отображения базовое содержимое веб-страницы, хотя он обеспечивает только самые мощные браузеры для отображения форматированное содержимое.</span><span class="sxs-lookup"><span data-stu-id="94882-226">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="94882-227">JavaScript и CSS, включенные в файлы jQuery мобильного стиля помочь браузеры мобильных устройств в соответствии с содержимым на экране без внесения изменений в разметке страницы.</span><span class="sxs-lookup"><span data-stu-id="94882-227">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="94882-229">*Библиотека jQuery мобильных, включенный в шаблон*</span><span class="sxs-lookup"><span data-stu-id="94882-229">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="94882-230">**Разметки на основе HTML5**</span><span class="sxs-lookup"><span data-stu-id="94882-230">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="94882-232">*Шаблон мобильного приложения, используя разметку HTML5, (Login.cshtml и index.cshtml)*</span><span class="sxs-lookup"><span data-stu-id="94882-232">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="94882-233">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="94882-233">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="94882-234">Откройте **Windows Phone 7 Emulator**.</span><span class="sxs-lookup"><span data-stu-id="94882-234">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="94882-235">На начальном экране телефона откройте Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="94882-235">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="94882-236">Ознакомьтесь с URL-адрес, где запуска приложения рабочего стола и перейдите по этому АДРЕСУ с телефона (например `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="94882-236">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="94882-237">Теперь вы сможете войти на страницу входа, или ознакомьтесь с возможностями о странице.</span><span class="sxs-lookup"><span data-stu-id="94882-237">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="94882-238">Обратите внимание на то, что стиль веб-сайта основана на новое приложение Metro для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="94882-238">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="94882-239">Шаблон проекта ASP.NET MVC 4 правильно отображается на мобильных устройствах, убедившись, что все элементы страницы были видимы и включены.</span><span class="sxs-lookup"><span data-stu-id="94882-239">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="94882-240">Обратите внимание, что ссылки на заголовок достаточно большим, чтобы быть щелкнул пользователь или.</span><span class="sxs-lookup"><span data-stu-id="94882-240">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="94882-241">![Проект шаблона страниц на мобильном устройстве](whats-new-in-aspnet-mvc-4/_static/image14.png "проекта шаблона страниц на мобильном устройстве")</span><span class="sxs-lookup"><span data-stu-id="94882-241">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="94882-242">*Страницы шаблонов проекта на мобильном устройстве*</span><span class="sxs-lookup"><span data-stu-id="94882-242">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="94882-243">Новый шаблон также использует **просмотра мета-тег**.</span><span class="sxs-lookup"><span data-stu-id="94882-243">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="94882-244">Большинство мобильных браузеров определить ширину окна виртуального браузера или &quot;окна просмотра&quot;, который больше, чем фактическую ширину мобильного устройства.</span><span class="sxs-lookup"><span data-stu-id="94882-244">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="94882-245">Это позволяет браузеры мобильных устройств для отображения всей веб-страницы внутри виртуального дисплея.</span><span class="sxs-lookup"><span data-stu-id="94882-245">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="94882-246">**Просмотра мета-тег** позволяет веб-разработчикам задать ширину, высоту и масштабирования области браузера на мобильных устройствах **.**</span><span class="sxs-lookup"><span data-stu-id="94882-246">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="94882-247">Шаблон ASP.NET MVC 4 для мобильных приложений задает окно просмотра по ширине устройства (&quot;width = ширина устройства&quot;) в шаблоне макета (*Views\Shared\_Layout.cshtml*), так что все страницы будет иметь их просмотра присвоено ширину экрана устройства.</span><span class="sxs-lookup"><span data-stu-id="94882-247">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="94882-248">Обратите внимание на то, что окно просмотра мета-тег не изменится в представление браузера по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="94882-248">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="94882-249">Откройте  **\_Layout.cshtml**, расположенного в **представления | Общие** папки и комментарий просмотра мета-тег.</span><span class="sxs-lookup"><span data-stu-id="94882-249">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="94882-250">Запустите приложение, в противном случае уже открыт и извлеките различия.</span><span class="sxs-lookup"><span data-stu-id="94882-250">Run the application, if not already opened, and check out the differences.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

<span data-ttu-id="94882-251">![Сайт после комментирование мета-тег просмотра](whats-new-in-aspnet-mvc-4/_static/image15.png "на сайт после комментирование мета-тег окна просмотра")</span><span class="sxs-lookup"><span data-stu-id="94882-251">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

<span data-ttu-id="94882-252">*Сайт после комментирование мета-тег окна просмотра*</span><span class="sxs-lookup"><span data-stu-id="94882-252">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="94882-253">В Visual Studio нажмите клавишу **SHIFT** + **F5** остановить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-253">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="94882-254">Раскомментируйте мета-тег окна просмотра.</span><span class="sxs-lookup"><span data-stu-id="94882-254">Uncomment the viewport meta tag.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="94882-255">Задача 3 - использование адаптивной отрисовки</span><span class="sxs-lookup"><span data-stu-id="94882-255">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="94882-256">В этой задаче вы узнаете, как другой метод для отображения веб-страницы правильно на мобильных устройствах и браузерах, в то же время, без какой-либо настройки.</span><span class="sxs-lookup"><span data-stu-id="94882-256">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="94882-257">Вы уже использовали просмотра мета-тег с одинаковой цели.</span><span class="sxs-lookup"><span data-stu-id="94882-257">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="94882-258">Теперь надо бороться с другой мощный метод: *адаптивной отрисовки*.</span><span class="sxs-lookup"><span data-stu-id="94882-258">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="94882-259">Адаптивная визуализация — это метод, который использует **медиа-запросами CSS3** настроить стиль, применяемый к странице.</span><span class="sxs-lookup"><span data-stu-id="94882-259">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="94882-260">Медиа-запросами определить условия внутри таблицы стилей, группирование стили CSS при определенном условии.</span><span class="sxs-lookup"><span data-stu-id="94882-260">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="94882-261">Только в том случае, если условие истинно, стиль применяется к объявленным объекты.</span><span class="sxs-lookup"><span data-stu-id="94882-261">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="94882-262">Гибкие возможности, предоставляемые методика адаптивной отрисовки включает все настройки для отображения сайта на разных устройствах.</span><span class="sxs-lookup"><span data-stu-id="94882-262">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="94882-263">Вы можете определить столько стили на отдельную таблицу стилей без написания кода логики в любое выбрать стиль.</span><span class="sxs-lookup"><span data-stu-id="94882-263">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="94882-264">Таким образом это очень ловко способ адаптации стилей страницы, поскольку он уменьшает объем дублирование кода и логики для рендеринга.</span><span class="sxs-lookup"><span data-stu-id="94882-264">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="94882-265">С другой стороны использование пропускной способности возрастет, а немногим удается увеличить размер файлов CSS.</span><span class="sxs-lookup"><span data-stu-id="94882-265">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="94882-266">При использовании метода адаптивной отрисовки ваш сайт будет **отображаться неправильно, независимо от браузера.**</span><span class="sxs-lookup"><span data-stu-id="94882-266">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="94882-267">Тем не менее, следует, если пропускная способность дополнительной нагрузки имеет значения.</span><span class="sxs-lookup"><span data-stu-id="94882-267">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="94882-268">— Базовый формат носителя запроса: @media \[область: все | handheld | печати | проекции | экрана\] ([свойство: значение] и... [свойство: значение])</span><span class="sxs-lookup"><span data-stu-id="94882-268">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="94882-269">Примеры запросов мультимедиа: &gt;  <strong>@media все и (максимальная ширина: 1000px) и (min нулевой ширины: 700px) {}:</strong> для всех разрешений между 700px и 1000px.</span><span class="sxs-lookup"><span data-stu-id="94882-269">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="94882-270"><strong>@media экран и (min нулевой ширины: 400 пикселей) и (максимальная ширина: 700px) {...}:</strong> только для экранов.</span><span class="sxs-lookup"><span data-stu-id="94882-270"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="94882-271">Разрешение должно быть от 400 до 700px.</span><span class="sxs-lookup"><span data-stu-id="94882-271">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="94882-272"><strong>@media портативные и (min нулевой ширины: 20em), экран и (min нулевой ширины: 20em) {...}:</strong> для экранов и карманных устройств (mobile и устройств).</span><span class="sxs-lookup"><span data-stu-id="94882-272"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="94882-273">Минимальная ширина должна быть больше 20em.</span><span class="sxs-lookup"><span data-stu-id="94882-273">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="94882-274">Дополнительные сведения об этом можно найти на [сайт консорциума W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="94882-274">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="94882-275">Теперь будет изучено, как работает адаптивной отрисовки, повышения удобочитаемости ASP.NET MVC 4 по умолчанию шаблона веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="94882-275">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="94882-276">Откройте **PhotoGallery.sln** решения, вы создали в задаче 1 и выберите **PhotoGallery** проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-276">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="94882-277">Нажмите клавишу **F5** для запуска решения.</span><span class="sxs-lookup"><span data-stu-id="94882-277">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="94882-278">Изменение ширины браузера, настройку windows на половину или меньше, чем квартал от его исходного размера.</span><span class="sxs-lookup"><span data-stu-id="94882-278">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="94882-279">Обратите внимание на то, что происходит с элементами в заголовке: некоторые элементы не будут отображаться в видимой области заголовка.</span><span class="sxs-lookup"><span data-stu-id="94882-279">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="94882-280">Откройте <strong>Site.css</strong> файл из обозревателя решений Visual Studio, находятся в <strong>содержимого</strong> папки проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-280">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="94882-281">Нажмите клавишу <strong>CTRL + F</strong> открытия Visual Studio интегрированный поиск, и записи <strong>@media</strong> искомого <strong>медиа-запросов CSS</strong>.</span><span class="sxs-lookup"><span data-stu-id="94882-281">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="94882-282">Условие запроса мультимедиа, определенные в этот шаблон работает таким образом: если меньше размер окна браузера **850 пикселей**, применены правила CSS, они определены внутри этого блока мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="94882-282">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="94882-283">![Поиск запросов мультимедиа](whats-new-in-aspnet-mvc-4/_static/image16.png "поиск запрос носителя")</span><span class="sxs-lookup"><span data-stu-id="94882-283">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="94882-284">*Поиск запросов мультимедиа*</span><span class="sxs-lookup"><span data-stu-id="94882-284">*Locating the media query*</span></span>
4. <span data-ttu-id="94882-285">Замените значение атрибута Максимальная ширина, заданное в 850 пикселей с **10px**, чтобы отключить адаптивной отрисовки и нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="94882-285">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="94882-286">Вернитесь в браузер и нажмите клавишу **CTRL + F5** для обновления страницы с внесенные изменения.</span><span class="sxs-lookup"><span data-stu-id="94882-286">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="94882-287">Обратите внимание различия в любой из страниц, при изменении ширины окна.</span><span class="sxs-lookup"><span data-stu-id="94882-287">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="94882-288">![В левой части страницы применяет @media стиль, в правой части стиль опущен](whats-new-in-aspnet-mvc-4/_static/image17.png "применяет в левой части страницы @media стиль, в правой части стиль опущен")</span><span class="sxs-lookup"><span data-stu-id="94882-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="94882-289"><em>В левой части страницы применяет @media опущен стиля в правом углу: стиль</em></span><span class="sxs-lookup"><span data-stu-id="94882-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="94882-290">Теперь давайте проверим, что происходит на мобильных устройствах:</span><span class="sxs-lookup"><span data-stu-id="94882-290">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="94882-291">![В левой части страницы применяет @media стиль, в правой части стиль опущен](whats-new-in-aspnet-mvc-4/_static/image18.png "применяет в левой части страницы @media стиль, в правой части стиль опущен")</span><span class="sxs-lookup"><span data-stu-id="94882-291">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="94882-292"><em>В левой части страницы применяет @media опущен стиля в правом углу: стиль</em></span><span class="sxs-lookup"><span data-stu-id="94882-292"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="94882-293">Несмотря на то, что вы заметите, что изменения при просмотре страницы в веб-браузер не очень важно при использовании мобильного устройства различия становятся более очевидными.</span><span class="sxs-lookup"><span data-stu-id="94882-293">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="94882-294">В левой области мы видим, что пользовательский стиль повышает удобство чтения.</span><span class="sxs-lookup"><span data-stu-id="94882-294">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="94882-295">Адаптивная отрисовка может использоваться в гораздо больше сценариев, что упрощает применение условного стиля на веб-сайт и устранению распространенных проблем стиля с помощью подхода, чудесно.</span><span class="sxs-lookup"><span data-stu-id="94882-295">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="94882-296">Окна просмотра мета-тег и медиа-запросов CSS не относящиеся к ASP.NET MVC 4, позволяет вам воспользоваться преимуществами этих функций в любое веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="94882-296">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="94882-297">В Visual Studio нажмите клавишу **SHIFT** + **F5** остановить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-297">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="94882-298">Упражнение 2: Создание веб-приложение фотоальбома</span><span class="sxs-lookup"><span data-stu-id="94882-298">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="94882-299">В этом упражнении будет работать в приложение фотоальбома для отображения фотографий.</span><span class="sxs-lookup"><span data-stu-id="94882-299">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="94882-300">Мы начнем с шаблона проекта ASP.NET MVC 4, и затем нужно добавить функцию для получения фотографий из службы и их отображения на домашней странице.</span><span class="sxs-lookup"><span data-stu-id="94882-300">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="94882-301">В следующем упражнении вы обновите это решение, чтобы улучшить способ отображения на мобильных устройствах.</span><span class="sxs-lookup"><span data-stu-id="94882-301">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="94882-302">Задача 1 - Создание службы макетов фото</span><span class="sxs-lookup"><span data-stu-id="94882-302">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="94882-303">В этой задаче вы создадите макет из службы хранения фотографий, для получения содержимого, которое будет отображаться в коллекции.</span><span class="sxs-lookup"><span data-stu-id="94882-303">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="94882-304">Чтобы сделать это, вы добавите новый контроллер, который просто возвращает JSON-файл с данными каждой фотографии.</span><span class="sxs-lookup"><span data-stu-id="94882-304">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="94882-305">Откройте **Visual Studio** Если еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="94882-305">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="94882-306">Выберите **файл | Новые | Проект** команды меню.</span><span class="sxs-lookup"><span data-stu-id="94882-306">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="94882-307">В **новый проект** диалоговом окне выберите **Visual C# | Web** шаблона на левой панели дерева, а затем выберите **веб-приложение ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="94882-307">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="94882-308">Назовите проект **PhotoGallery**, выберите расположение (или оставьте значение по умолчанию) и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-308">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="94882-309">Кроме того, вы можете продолжать работать с вашей существующей ASP.NET MVC 4 **веб-приложение** решение из **упражнения 1** и пропустите следующий шаг.</span><span class="sxs-lookup"><span data-stu-id="94882-309">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="94882-310">В **создания проекта ASP.NET MVC 4** выберите **веб-приложение** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-310">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="94882-311">Убедитесь, что у вас есть выбранные в качестве обработчика представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="94882-311">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="94882-312">В **обозревателе решений**, щелкните правой кнопкой мыши **приложения\_данных** папку проекта и выберите **Add | Существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="94882-312">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="94882-313">Перейдите к **Source\Assets\App\_данных** папку данную лабораторию и добавьте **Photos.json** файла.</span><span class="sxs-lookup"><span data-stu-id="94882-313">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="94882-314">Создайте новый контроллер с именем **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="94882-314">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="94882-315">Чтобы сделать это, щелкните правой кнопкой мыши **контроллеров** перейдите к папке **добавить** и выберите **контроллера.**</span><span class="sxs-lookup"><span data-stu-id="94882-315">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="94882-316">Полное имя контроллера, оставьте **пустой контроллер MVC** шаблона и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="94882-316">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="94882-317">![Добавление PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Добавление PhotoController")</span><span class="sxs-lookup"><span data-stu-id="94882-317">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="94882-318">*Добавление PhotoController*</span><span class="sxs-lookup"><span data-stu-id="94882-318">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="94882-319">Замените **индекс** следующим кодом **коллекции** действие и возвращают содержимое из файла JSON, недавно добавленные в проект.</span><span class="sxs-lookup"><span data-stu-id="94882-319">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="94882-320">(Code Snippet - *действие коллекции ASP.NET MVC 4 лаборатории - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="94882-320">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="94882-321">Нажмите клавишу **F5** для запуска решения, а затем перейдите в следующий URL-адрес для тестирования службы макеты photo: `http://localhost:[port]/photo/gallery` ([порт] значение зависит от текущего порта, где было запущено приложение).</span><span class="sxs-lookup"><span data-stu-id="94882-321">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="94882-322">Запрос на этот URL-адрес следует извлекать содержимое **Photos.json** файла.</span><span class="sxs-lookup"><span data-stu-id="94882-322">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="94882-323">![Проверка службы макеты photo](whats-new-in-aspnet-mvc-4/_static/image20.png "тестирование службы макеты фото")</span><span class="sxs-lookup"><span data-stu-id="94882-323">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="94882-324">*Проверка службы макеты фото*</span><span class="sxs-lookup"><span data-stu-id="94882-324">*Testing the mocked photo service*</span></span>

<span data-ttu-id="94882-325">В настоящей реализации можно было использовать [веб-API ASP.NET](../../../../web-api/index.md) для реализации службы Photo Gallery.</span><span class="sxs-lookup"><span data-stu-id="94882-325">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="94882-326">Веб-API ASP.NET — это платформа, которая позволяет легко создавать HTTP-службы для широкого диапазона клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="94882-326">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="94882-327">ASP.NET Web API - это идеальная платформа для сборки REST-приложений на базе .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="94882-327">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="94882-328">Задача 2 - отображение Photo Gallery</span><span class="sxs-lookup"><span data-stu-id="94882-328">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="94882-329">В этой задаче будет обновлять домашнюю страницу, чтобы показать коллекцию фотографий с помощью макеты службы, созданной в первой задаче данного упражнения.</span><span class="sxs-lookup"><span data-stu-id="94882-329">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="94882-330">Будет добавлять файлы модели и обновлять представления коллекции.</span><span class="sxs-lookup"><span data-stu-id="94882-330">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="94882-331">В Visual Studio нажмите клавишу **SHIFT** + **F5** остановить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-331">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="94882-332">Создание **Photo** в класс **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-332">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="94882-333">Чтобы сделать это, щелкните правой кнопкой мыши **моделей** папку, выберите **добавить** и нажмите кнопку **класс**.</span><span class="sxs-lookup"><span data-stu-id="94882-333">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="94882-334">Затем задайте имя **Photo.cs** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="94882-334">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="94882-335">Добавьте следующие члены для **Photo** класса.</span><span class="sxs-lookup"><span data-stu-id="94882-335">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="94882-336">(Code Snippet - *модели ASP.NET MVC 4 лаборатории - Ex02 - Photo*)</span><span class="sxs-lookup"><span data-stu-id="94882-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="94882-337">Откройте файл **HomeController.cs** в папке **Controllers**.</span><span class="sxs-lookup"><span data-stu-id="94882-337">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="94882-338">Добавьте следующие инструкции using.</span><span class="sxs-lookup"><span data-stu-id="94882-338">Add the following using statements.</span></span>

    <span data-ttu-id="94882-339">(Code Snippet - *директивы using HomeController лаборатории - Ex02 - ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="94882-339">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="94882-340">Обновление **индекс** действие, чтобы использовать **HttpClient** для получения коллекции данных, а затем используйте **JavaScriptSerializer** для десериализации для модели представления.</span><span class="sxs-lookup"><span data-stu-id="94882-340">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="94882-341">(Code Snippet - *действие индекса ASP.NET MVC 4 лаборатории - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="94882-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="94882-342">Откройте **Index.cshtml** файла, расположенного в **Views\Home** папку и замените все содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="94882-342">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="94882-343">Этот код перебирает все фотографии, полученных от службы и отображает их в неупорядоченный список.</span><span class="sxs-lookup"><span data-stu-id="94882-343">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="94882-344">(Code Snippet - *списка фотографий ASP.NET MVC 4 лаборатории - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="94882-344">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="94882-345">В **обозревателе решений**, щелкните правой кнопкой мыши **содержимого** папку проекта и выберите **Add | Существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="94882-345">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="94882-346">Перейдите к **Source\Assets\Content** папку данную лабораторию и добавьте **Site.css** файл.</span><span class="sxs-lookup"><span data-stu-id="94882-346">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="94882-347">Необходимо будет подтвердить его замены.</span><span class="sxs-lookup"><span data-stu-id="94882-347">You will have to confirm its replacement.</span></span> <span data-ttu-id="94882-348">Если у вас есть **Site.css** файл открыт, необходимо будет подтвердить на перезагрузку файла также.</span><span class="sxs-lookup"><span data-stu-id="94882-348">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="94882-349">Откройте проводник и скопировать весь **фотографии** папка размещена в **Source\Assets** папку данную лабораторию в корневую папку проекта в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="94882-349">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="94882-350">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="94882-350">Run the application.</span></span> <span data-ttu-id="94882-351">Теперь вы увидите домашнюю страницу, отображения фотографий в коллекции.</span><span class="sxs-lookup"><span data-stu-id="94882-351">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="94882-352">![Фотоальбом](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span><span class="sxs-lookup"><span data-stu-id="94882-352">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="94882-353">*Фотоальбом*</span><span class="sxs-lookup"><span data-stu-id="94882-353">*Photo Gallery*</span></span>
11. <span data-ttu-id="94882-354">В Visual Studio нажмите клавишу **SHIFT** + **F5** остановить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-354">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="94882-355">Упражнение 3: Добавление поддержки для мобильных устройств</span><span class="sxs-lookup"><span data-stu-id="94882-355">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="94882-356">Одно из ключевых обновлений в ASP.NET MVC 4 поддерживается для разработки мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="94882-356">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="94882-357">В этом упражнении вы изучить новые функции ASP.NET MVC 4 для мобильных приложений путем расширения решения Коллекция фотографий, созданный в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="94882-357">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="94882-358">Задача 1 - Установка jQuery Mobile, в приложении ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="94882-358">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="94882-359">Откройте **начать** решений, расположенный **источника/Ex3-MobileSupport/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-359">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="94882-360">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="94882-360">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="94882-361">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="94882-361">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="94882-362">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="94882-362">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="94882-363">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="94882-363">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="94882-364">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="94882-364">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="94882-365">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-365">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="94882-366">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-366">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="94882-367">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="94882-367">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="94882-368">Откройте **консоль диспетчера пакетов** , щелкнув **средства** &gt; **диспетчер пакетов библиотеки** &gt; **диспетчера пакетов Консоль** пункт меню.</span><span class="sxs-lookup"><span data-stu-id="94882-368">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="94882-369">![Открытие консоли диспетчера пакетов NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "при открытии консоли диспетчера пакетов NuGet")</span><span class="sxs-lookup"><span data-stu-id="94882-369">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="94882-370">*Открытие консоли диспетчера пакетов NuGet*</span><span class="sxs-lookup"><span data-stu-id="94882-370">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="94882-371">В консоли диспетчера пакетов выполните следующую команду, чтобы установить **jQuery.Mobile.MVC** пакета.</span><span class="sxs-lookup"><span data-stu-id="94882-371">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="94882-372">jQuery Mobile — это библиотека открытым исходным кодом для создания веб-оптимизированных для сенсорных экранов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="94882-372">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="94882-373">Пакет NuGet jQuery.Mobile.MVC включает вспомогательные методы для использования с приложением ASP.NET MVC 4 jQuery Mobile.</span><span class="sxs-lookup"><span data-stu-id="94882-373">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-374">С помощью следующей команды будут загружать jQuery.Mobile.MVC библиотеки из Nuget.</span><span class="sxs-lookup"><span data-stu-id="94882-374">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="94882-375">по полудню</span><span class="sxs-lookup"><span data-stu-id="94882-375">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="94882-376">Эта команда устанавливает jQuery Mobile и некоторые вспомогательные файлы, включая следующие:</span><span class="sxs-lookup"><span data-stu-id="94882-376">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="94882-377">**Views/Shared/\_Layout.Mobile.cshtml**: — макет на базе мобильных технологий jQuery, оптимизированный для небольших экранов.</span><span class="sxs-lookup"><span data-stu-id="94882-377">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="94882-378">Если веб-сайт получает запрос из браузера мобильного устройства, он заменит исходный макет (\_Layout.cshtml) с данным элементом.</span><span class="sxs-lookup"><span data-stu-id="94882-378">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="94882-379">Компонент переключателя: состоит из **Views/Shared/\_ViewSwitcher.cshtml** частичного представления и **ViewSwitcherController.cs** контроллера.</span><span class="sxs-lookup"><span data-stu-id="94882-379">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="94882-380">Этот компонент будет отображать ссылку в браузерах мобильных устройств, чтобы пользователи могли переключиться в настольной версии страницы.</span><span class="sxs-lookup"><span data-stu-id="94882-380">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="94882-381">![Photo Gallery проекта с поддержкой мобильных](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery проекта с помощью поддержка мобильных устройств")</span><span class="sxs-lookup"><span data-stu-id="94882-381">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="94882-382">*Проект фотоальбома с поддержка мобильных устройств*</span><span class="sxs-lookup"><span data-stu-id="94882-382">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="94882-383">Регистрация мобильных устройств пакеты.</span><span class="sxs-lookup"><span data-stu-id="94882-383">Register the Mobile bundles.</span></span> <span data-ttu-id="94882-384">Чтобы сделать это, откройте **Global.asax.cs** файл и добавьте следующую строку.</span><span class="sxs-lookup"><span data-stu-id="94882-384">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="94882-385">(Code Snippet - *мобильных пакеты ASP.NET MVC 4 лаборатории - Ex03 - Register*)</span><span class="sxs-lookup"><span data-stu-id="94882-385">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="94882-386">Запустите приложение с помощью веб-браузер.</span><span class="sxs-lookup"><span data-stu-id="94882-386">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="94882-387">Откройте **Windows Phone 7 Emulator,** в **меню «Пуск» | Все программы | Windows Phone SDK 7.1 | Эмулятор Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="94882-387">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="94882-388">На начальном экране телефона откройте Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="94882-388">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="94882-389">Ознакомьтесь с URL-адрес, где запустить приложение и перейдите по этому АДРЕСУ с помощью обозревателя телефона (например `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="94882-389">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="94882-390">Обратите внимание, что приложение будет выглядеть иначе в эмуляторе Windows Phone, как jQuery.Mobile.MVC создал новые ресурсы в проекте, представления, оптимизированный для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="94882-390">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="94882-391">Обратите внимание, что сообщение в верхней части телефона, показывающая ссылку, которая переключается в представление рабочего стола.</span><span class="sxs-lookup"><span data-stu-id="94882-391">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="94882-392">Кроме того  **\_Layout.Mobile.cshtml** макет, который был создан с помощью пакета установки — включая другой макет в приложении.</span><span class="sxs-lookup"><span data-stu-id="94882-392">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-393">На данный момент нет ссылки для возврата к мобильному представлению.</span><span class="sxs-lookup"><span data-stu-id="94882-393">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="94882-394">Он будет включен в более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="94882-394">It will be included in later versions.</span></span>

    <span data-ttu-id="94882-395">![Представление для мобильных устройств Photo коллекции домашней страницы](whats-new-in-aspnet-mvc-4/_static/image24.png "мобильного представления коллекции фотографий на домашней странице")</span><span class="sxs-lookup"><span data-stu-id="94882-395">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="94882-396">*Представление коллекции фотографий на домашней странице для мобильных устройств*</span><span class="sxs-lookup"><span data-stu-id="94882-396">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="94882-397">В Visual Studio нажмите клавишу **SHIFT** + **F5** остановить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-397">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="94882-398">Задача 2 - Создание мобильных представлений</span><span class="sxs-lookup"><span data-stu-id="94882-398">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="94882-399">В этой задаче вы создадите мобильной версии в представление index с содержимым, адаптирован для повышения appareance на мобильных устройствах.</span><span class="sxs-lookup"><span data-stu-id="94882-399">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="94882-400">Копировать **Views\Home\Index.cshtml** просмотра, а затем вставьте его для создания копии, переименуйте новый файл в **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="94882-400">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="94882-401">Открытие нового созданного **Index.Mobile.cshtml** просмотра и замените существующий &lt;ul&gt; тега с этим кодом.</span><span class="sxs-lookup"><span data-stu-id="94882-401">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="94882-402">Таким образом происходит обновление &lt;ul&gt; тегов с заметками мобильных данных jQuery для использования мобильных темы с jQuery.</span><span class="sxs-lookup"><span data-stu-id="94882-402">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="94882-403">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="94882-403">Notice that:</span></span>
    > 
    > - <span data-ttu-id="94882-404">**Роль данных** атрибут **listview** визуализирует список при помощи стилей listview.</span><span class="sxs-lookup"><span data-stu-id="94882-404">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="94882-405">**Вставки данных** атрибута задано значение true будет отображать список с границу со скругленными углами и полей.</span><span class="sxs-lookup"><span data-stu-id="94882-405">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="94882-406">**Фильтр данных** атрибут **true** создаст поле поиска.</span><span class="sxs-lookup"><span data-stu-id="94882-406">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="94882-407">Дополнительные сведения о правилах мобильных jQuery в документацию по проекту. [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="94882-407">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="94882-408">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="94882-408">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="94882-409">Переключиться в режим **эмулятора Windows Phone** и обновите узел.</span><span class="sxs-lookup"><span data-stu-id="94882-409">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="94882-410">Обратите внимание, что благодаря новому интерфейсу списке коллекции, а также новое поле поиска, расположенную в верхней.</span><span class="sxs-lookup"><span data-stu-id="94882-410">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="94882-411">Введите слово в поле поиска (например, **Tulips**) для начала поиска в коллекции фотографий.</span><span class="sxs-lookup"><span data-stu-id="94882-411">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="94882-412">![Коллекции с помощью стиля listview с фильтрацией](whats-new-in-aspnet-mvc-4/_static/image25.png "коллекции с помощью стиля listview с фильтрацией")</span><span class="sxs-lookup"><span data-stu-id="94882-412">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="94882-413">*Коллекции с помощью стиля listview с фильтрацией*</span><span class="sxs-lookup"><span data-stu-id="94882-413">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="94882-414">Таким образом, вы использовали Mobilizer представление рецепт для создания копии индекс представления с &quot;мобильных&quot; суффикс.</span><span class="sxs-lookup"><span data-stu-id="94882-414">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="94882-415">Этот суффикс указывает ASP.NET MVC 4, что все запросы, созданные с помощью мобильного устройства будет использовать эта копия индекса.</span><span class="sxs-lookup"><span data-stu-id="94882-415">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="94882-416">Кроме того вы обновили мобильную версию индекс представления, чтобы использовать jQuery Mobile для совершенствования внешний вид сайта на мобильных устройствах.</span><span class="sxs-lookup"><span data-stu-id="94882-416">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="94882-417">Вернитесь в Visual Studio и откройте **Site.Mobile.css** каталоге **содержимого** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-417">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="94882-418">Исправьте положение Название фотографии, чтобы отобразить в правой части изображения.</span><span class="sxs-lookup"><span data-stu-id="94882-418">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="94882-419">Чтобы сделать это, добавьте следующий код, чтобы **Site.Mobile.css** файла.</span><span class="sxs-lookup"><span data-stu-id="94882-419">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="94882-420">CSS</span><span class="sxs-lookup"><span data-stu-id="94882-420">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="94882-421">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="94882-421">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="94882-422">Вернитесь в **эмулятора Windows Phone** и обновите узел.</span><span class="sxs-lookup"><span data-stu-id="94882-422">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="94882-423">Обратите внимание, что теперь удачно позиционируется в Название фотографии.</span><span class="sxs-lookup"><span data-stu-id="94882-423">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="94882-424">![Заголовок располагается в правой части изображения](whats-new-in-aspnet-mvc-4/_static/image26.png "Title, расположенный в правой части изображения")</span><span class="sxs-lookup"><span data-stu-id="94882-424">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="94882-425">*Заголовок располагается в правой части изображения*</span><span class="sxs-lookup"><span data-stu-id="94882-425">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="94882-426">Задача 3 - jQuery Mobile темы</span><span class="sxs-lookup"><span data-stu-id="94882-426">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="94882-427">Каждый макет и мини-приложения на jQuery Mobile разработано на основе новой объектно ориентированного CSS платформы, дает возможность применить тему полный единой визуального проектирования к сайтам и приложениям.</span><span class="sxs-lookup"><span data-stu-id="94882-427">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="94882-428">jQuery Mobile темы по умолчанию включает в себя 5 образцы, на которые указывает буквы (a, b, c, d, e) краткий справочник.</span><span class="sxs-lookup"><span data-stu-id="94882-428">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="94882-429">В этой задаче вы обновите то мобильный макет, чтобы использовать другую тему по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="94882-429">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="94882-430">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-430">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="94882-431">Откройте  **\_Layout.Mobile.cshtml** файл, расположенный в **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="94882-431">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="94882-432">Найдите элемент div с роли данных, равным &quot;страницы&quot; и обновить **data-theme** для &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-432">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="94882-433">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="94882-433">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="94882-434">Обновите сайт в **эмулятора Windows Phone** и обратите внимание, что новая схема цветов.</span><span class="sxs-lookup"><span data-stu-id="94882-434">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="94882-435">![Макета для мобильных устройств с помощью другой цветовой схемы](whats-new-in-aspnet-mvc-4/_static/image27.png "макета для мобильных устройств с помощью другой цветовой схемы")</span><span class="sxs-lookup"><span data-stu-id="94882-435">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="94882-436">*С другой цветовой схемы макета для мобильных устройств*</span><span class="sxs-lookup"><span data-stu-id="94882-436">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="94882-437">Задача 4 - с помощью компонента переключателя и браузер переопределение функций</span><span class="sxs-lookup"><span data-stu-id="94882-437">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="94882-438">Соглашение для веб-страниц, оптимизированных для мобильных устройств — Добавление ссылки которых указано что-то как представление рабочего стола или режиме весь сайт, который позволяет пользователям переключиться в настольной версии страницы.</span><span class="sxs-lookup"><span data-stu-id="94882-438">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="94882-439">Пакет jQuery.Mobile.MVC включает пример **-переключателя** компонент для этой цели, используемых в  **\_Layout.Mobile.cshtml** представления.</span><span class="sxs-lookup"><span data-stu-id="94882-439">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="94882-440">![Ссылку, чтобы переключиться в представление рабочего стола](whats-new-in-aspnet-mvc-4/_static/image28.png "ссылку, чтобы переключиться в представление рабочего стола")</span><span class="sxs-lookup"><span data-stu-id="94882-440">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="94882-441">*Ссылку, чтобы перейти в представление рабочего стола*</span><span class="sxs-lookup"><span data-stu-id="94882-441">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="94882-442">Просмотреть переключатель используется новая функция, называемая **переопределения браузера**.</span><span class="sxs-lookup"><span data-stu-id="94882-442">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="94882-443">Эта функция позволяет приложению рассматривать запросы, как если бы они поступали от другой браузер (агент пользователя), теперь поступают с чем.</span><span class="sxs-lookup"><span data-stu-id="94882-443">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="94882-444">В этой задаче будет изучено пример реализации представления переключателя добавленные jQuery.Mobile.MVC и новый обозреватель переопределение функций в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="94882-444">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="94882-445">Переключитесь в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-445">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="94882-446">Откройте  **\_Layout.Mobile.cshtml** представление, расположенный в **Views\Shared** папку и обратите внимание, что вызываемый как частичное представление компонента представления переключателя.</span><span class="sxs-lookup"><span data-stu-id="94882-446">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="94882-447">![Макета для мобильных устройств с помощью компонента переключателя](whats-new-in-aspnet-mvc-4/_static/image29.png "макета для мобильных устройств с помощью переключателя компонента")</span><span class="sxs-lookup"><span data-stu-id="94882-447">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="94882-448">*С помощью компонента переключателя макета для мобильных устройств*</span><span class="sxs-lookup"><span data-stu-id="94882-448">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="94882-449">Откройте  **\_ViewSwitcher.cshtml** частичного представления.</span><span class="sxs-lookup"><span data-stu-id="94882-449">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="94882-450">Частичное представление использует новый метод **ViewContext.HttpContext.GetOverriddenBrowser()** для определения источника веб-запроса и отображения соответствующую ссылку, чтобы перейти либо к представлениям Desktop или Mobile.</span><span class="sxs-lookup"><span data-stu-id="94882-450">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="94882-451">**GetOverridenBrowser** возвращает **HttpBrowserCapabilitiesBase** , соответствующего агента пользователя, заданных в настоящее время для запроса (фактические или переопределено).</span><span class="sxs-lookup"><span data-stu-id="94882-451">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="94882-452">Это значение можно использовать для получения свойств, таких как **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="94882-452">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="94882-453">![Частичное представление ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher частичного представления")</span><span class="sxs-lookup"><span data-stu-id="94882-453">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="94882-454">*ViewSwitcher частичного представления*</span><span class="sxs-lookup"><span data-stu-id="94882-454">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="94882-455">Откройте **ViewSwitcherController.cs** класс находится в **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-455">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="94882-456">Ознакомьтесь с этой SwitchView действие вызывается в ссылке в компоненте ViewSwitcher и обратите внимание, что новые методы HttpContext.</span><span class="sxs-lookup"><span data-stu-id="94882-456">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="94882-457">**HttpContext.ClearOverridenBrowser()** метод удаляет все переопределенные агенты пользователя для текущего запроса.</span><span class="sxs-lookup"><span data-stu-id="94882-457">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="94882-458">**HttpContext.SetOverridenBrowser()** метод переопределяет значение агента пользователя запроса, с указанием указанного агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="94882-458">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="94882-459">![Контроллер ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher контроллера")</span><span class="sxs-lookup"><span data-stu-id="94882-459">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="94882-460">*ViewSwitcher контроллера*</span><span class="sxs-lookup"><span data-stu-id="94882-460">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="94882-461">Подмена браузера — это функция core ASP.NET MVC 4, в которой также доступны, даже если не установить пакет jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="94882-461">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="94882-462">Тем не менее эта функция затрагивает только представление макета и частичного представления, и он не влияет на функции, которые зависят от объекта Request.Browser.</span><span class="sxs-lookup"><span data-stu-id="94882-462">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="94882-463">Задача 5 - добавление переключателя в представлении рабочего стола</span><span class="sxs-lookup"><span data-stu-id="94882-463">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="94882-464">В этой задаче вы обновите макет рабочего стола для включения переключателя.</span><span class="sxs-lookup"><span data-stu-id="94882-464">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="94882-465">Это позволит мобильных пользователей, чтобы вернуться в представление для мобильных устройств при просмотре представление рабочего стола.</span><span class="sxs-lookup"><span data-stu-id="94882-465">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="94882-466">Обновите сайт в **эмулятора Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="94882-466">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="94882-467">Щелкните **представление рабочего стола** ссылку в верхней части коллекции.</span><span class="sxs-lookup"><span data-stu-id="94882-467">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="94882-468">Обратите внимание, что в представлении рабочего стола, чтобы разрешить вернуться к представление для мобильных устройств не представление переключателя.</span><span class="sxs-lookup"><span data-stu-id="94882-468">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="94882-469">Вернитесь в Visual Studio и откройте  **\_Layout.cshtml** представления.</span><span class="sxs-lookup"><span data-stu-id="94882-469">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="94882-470">Найдите раздел входа и вставьте вызов для подготовки к просмотру  **\_ViewSwitcher** частичного представления ниже  **\_LogOnPartial** частичного представления.</span><span class="sxs-lookup"><span data-stu-id="94882-470">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="94882-471">Затем нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="94882-471">Then, press **CTRL + S** to save the changes.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="94882-472">Нажмите клавишу **CTRL + S** для сохранения изменений.</span><span class="sxs-lookup"><span data-stu-id="94882-472">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="94882-473">Обновите страницу в эмуляторе Windows Phone и дважды щелкните для увеличения изображения на экране.</span><span class="sxs-lookup"><span data-stu-id="94882-473">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="94882-474">Обратите внимание, что теперь отображаются на домашней странице **мобильного представления** ссылку, которая переключается от мобильного телефона в представление рабочего стола.</span><span class="sxs-lookup"><span data-stu-id="94882-474">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="94882-475">![Просмотреть переключатель, подготавливается к просмотру в представление рабочего стола](whats-new-in-aspnet-mvc-4/_static/image32.png "переключателя подготавливается к просмотру в представление рабочего стола")</span><span class="sxs-lookup"><span data-stu-id="94882-475">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="94882-476">*Просмотреть переключатель, подготавливается к просмотру в представление рабочего стола*</span><span class="sxs-lookup"><span data-stu-id="94882-476">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="94882-477">Снова вернитесь в представление для мобильных устройств и перейдите к <strong>о</strong> страницы (http://localhost[порт] / Home/о).</span><span class="sxs-lookup"><span data-stu-id="94882-477">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="94882-478">Обратите внимание на то, что, даже если вы не создавали в представлении About.Mobile.cshtml, Обзорная страница отображается с использованием макета для мобильных устройств (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="94882-478">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="94882-479">![Страница About](whats-new-in-aspnet-mvc-4/_static/image33.png "о странице")</span><span class="sxs-lookup"><span data-stu-id="94882-479">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="94882-480">*О странице*</span><span class="sxs-lookup"><span data-stu-id="94882-480">*About page*</span></span>
8. <span data-ttu-id="94882-481">Наконец откройте сайт в классических веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="94882-481">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="94882-482">Обратите внимание на то, что ни один из предыдущих обновлений изменяет представление рабочего стола.</span><span class="sxs-lookup"><span data-stu-id="94882-482">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="94882-483">![Коллекция фотографий представление рабочего стола](whats-new-in-aspnet-mvc-4/_static/image34.png "представление рабочего стола Коллекция фотографий")</span><span class="sxs-lookup"><span data-stu-id="94882-483">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="94882-484">*Коллекция фотографий представление рабочего стола*</span><span class="sxs-lookup"><span data-stu-id="94882-484">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="94882-485">Задача 6 - создание новых режимов отображения</span><span class="sxs-lookup"><span data-stu-id="94882-485">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="94882-486">Новая функция режимов отображения для работы приложения выберите представления, в зависимости от браузера, создающего запрос.</span><span class="sxs-lookup"><span data-stu-id="94882-486">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="94882-487">Например, если классический браузер запрашивает на домашнюю страницу, то приложение завершит работу **Views\Home\Index.cshtml** шаблона.</span><span class="sxs-lookup"><span data-stu-id="94882-487">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="94882-488">Затем, если браузер мобильного устройства запрашивает на домашнюю страницу, то приложение завершит работу **Views\Home\Index.mobile.cshtml** шаблона.</span><span class="sxs-lookup"><span data-stu-id="94882-488">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="94882-489">В этой задаче вы создадите настраиваемый макет для устройств iPhone и необходимо смоделировать запросы из устройства iPhone.</span><span class="sxs-lookup"><span data-stu-id="94882-489">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="94882-490">Чтобы сделать это, можно использовать либо эмулятор/эмулятор iPhone (как [электрического симулятор Mobile](http://www.electricplum.com/)) или браузера с помощью надстроек, которые изменяют агента пользователя.</span><span class="sxs-lookup"><span data-stu-id="94882-490">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="94882-491">Инструкции о том, как присвоить строку агента пользователя в обозревателе Safari для эмуляции iPhone, см. в разделе [как предоставить Safari представьте это IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) в блоге Дэвида Alison.</span><span class="sxs-lookup"><span data-stu-id="94882-491">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="94882-492">**Обратите внимание на то, что эта задача является необязательным, и можно продолжать в лаборатории. без ее выполнения.**</span><span class="sxs-lookup"><span data-stu-id="94882-492">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="94882-493">В Visual Studio нажмите клавишу **SHIFT** + **F5** остановить отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-493">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="94882-494">Откройте **Global.asax.cs** и добавьте следующий оператор using.</span><span class="sxs-lookup"><span data-stu-id="94882-494">Open **Global.asax.cs** and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="94882-495">Добавьте следующий выделенный код в приложение\_метод Start.</span><span class="sxs-lookup"><span data-stu-id="94882-495">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="94882-496">(Code Snippet - *ASP.NET MVC 4 лаборатории - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="94882-496">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

<span data-ttu-id="94882-497">Вы зарегистрировали новый **DefaultDisplayMode с именем &quot;iPhone&quot;**, в статический **DisplayModeProvider.Instance.Modes** статического списка, которая будет сопоставлена с Каждый входящий запрос.</span><span class="sxs-lookup"><span data-stu-id="94882-497">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="94882-498">Если входящий запрос содержит строку &quot;iPhone&quot;, ASP.NET MVC поиск представлений, имена которых содержат &quot;iPhone&quot; суффикс.</span><span class="sxs-lookup"><span data-stu-id="94882-498">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="94882-499">Параметр 0 указывает, как определенные новый режим; Например, это представление является более точным, чем Общие &quot;.mobile&quot; правило, которое соответствует запросам с мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="94882-499">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

<span data-ttu-id="94882-500">После выполнения этого кода, когда это браузер iPhone создает запрос, приложение будет использовать **Views\Shared\\_Layout.iPhone.cshtml** макет, вы создадите на следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="94882-500">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

> [!NOTE]
> <span data-ttu-id="94882-501">Такой способ тестирования запроса для iPhone был упрощен для демонстрационных целей и не может работать неправильно для каждой строки агента пользователя iPhone (для тестирования пример чувствительно к регистру).</span><span class="sxs-lookup"><span data-stu-id="94882-501">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>

4. <span data-ttu-id="94882-502">Создать копию  **\_Layout.Mobile.cshtml** файл **Views\Shared** папку и переименуйте копию &quot; **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-502">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="94882-503">Откройте  **\_Layout.iPhone.csthml** вы создали на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="94882-503">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="94882-504">Найдите элемент div с атрибутом роли данных, равным **страницы** и измените **data-theme** атрибут &quot;**a**&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-504">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

<span data-ttu-id="94882-505">Теперь у вас есть 3 макетов в приложении ASP.NET MVC 4:</span><span class="sxs-lookup"><span data-stu-id="94882-505">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

1. <span data-ttu-id="94882-506">**\_Layout.cshtml**: макет по умолчанию, используемый для браузеров настольных компьютеров.</span><span class="sxs-lookup"><span data-stu-id="94882-506">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
2. <span data-ttu-id="94882-507">**\_Layout.Mobile.cshtml**: макет по умолчанию, используемый для мобильных устройств.</span><span class="sxs-lookup"><span data-stu-id="94882-507">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
3. <span data-ttu-id="94882-508">**\_Layout.iPhone.cshtml**: определенный макет для устройства iPhone, используя другой цветовой схемы для различения \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="94882-508">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="94882-509">Нажмите клавишу **F5** для запуска приложения и перейдите на сайт в **эмулятора Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="94882-509">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="94882-510">Откройте **симулятора iPhone** (см. в разделе [приложении C](#AppendixC) инструкции по установке и настройке эмулятор iPhone) и перейдите на сайт слишком.</span><span class="sxs-lookup"><span data-stu-id="94882-510">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="94882-511">Обратите внимание на то, что каждый phone использует указанный шаблон.</span><span class="sxs-lookup"><span data-stu-id="94882-511">Notice that each phone is using the specific template.</span></span>

    ![Using-different-Views-For-Each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="94882-513">*С помощью различных представлений для каждого мобильного устройства*</span><span class="sxs-lookup"><span data-stu-id="94882-513">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="94882-514">Упражнение 4: Использование асинхронных контроллеров</span><span class="sxs-lookup"><span data-stu-id="94882-514">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="94882-515">Microsoft .NET Framework 4.5 предоставляет новые возможности языка в C# и Visual Basic, является основой новой асинхронность в программировании .NET.</span><span class="sxs-lookup"><span data-stu-id="94882-515">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="94882-516">Этот новый foundation делает асинхронное программирование, аналогичную - и примерно так же просто, как и - синхронное.</span><span class="sxs-lookup"><span data-stu-id="94882-516">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="94882-517">Теперь есть возможность написать асинхронные методы действия в ASP.NET MVC 4 с помощью **AsyncController** класса.</span><span class="sxs-lookup"><span data-stu-id="94882-517">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="94882-518">Асинхронные методы действия можно использовать для выполняющейся длительное время, не связаны с ЦП запросов.</span><span class="sxs-lookup"><span data-stu-id="94882-518">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="94882-519">Это предотвращает блокировку веб-сервера от выполнения операций во время обработки запроса.</span><span class="sxs-lookup"><span data-stu-id="94882-519">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="94882-520">AsyncController класс обычно используется для длительных вызовов веб-службы.</span><span class="sxs-lookup"><span data-stu-id="94882-520">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="94882-521">В этом упражнении рассматриваются основы асинхронной операции в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="94882-521">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="94882-522">Если вам необходимы более подробные сведения, вы можете ознакомиться со следующей статьей: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="94882-522">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="94882-523">Задача 1 - Реализация асинхронного контроллера</span><span class="sxs-lookup"><span data-stu-id="94882-523">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="94882-524">Откройте **начать** решений, расположенный **источника/Ex4-Async иBegin/** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-524">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="94882-525">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="94882-525">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="94882-526">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="94882-526">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="94882-527">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="94882-527">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="94882-528">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="94882-528">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="94882-529">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="94882-529">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="94882-530">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-530">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="94882-531">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="94882-531">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="94882-532">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="94882-532">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="94882-533">Откройте **HomeController.cs** класса из **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="94882-533">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="94882-534">Добавьте следующий оператор using.</span><span class="sxs-lookup"><span data-stu-id="94882-534">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="94882-535">Обновление **HomeController** наследование из класса **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="94882-535">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="94882-536">Контроллеры, являющиеся производными от AsyncController позволяют ASP.NET выполнять обработку асинхронных запросов, и они по-прежнему могут обслуживать синхронные методы действия.</span><span class="sxs-lookup"><span data-stu-id="94882-536">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="94882-537">Добавить **async** слово **индекс** метод и сделать его возвращаемый тип **задачи&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="94882-537">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="94882-538">**Async** ключевое слово является одним из .NET Framework 4.5 предоставляет новые ключевые слова; она сообщает компилятору, что этот метод содержит асинхронного кода.</span><span class="sxs-lookup"><span data-stu-id="94882-538">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="94882-539">Объект **задачи** представляет асинхронную операцию, которая может завершиться в некоторый момент в будущем.</span><span class="sxs-lookup"><span data-stu-id="94882-539">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="94882-540">Замените **клиента. GetAsync()** вызов с полной асинхронные версии с помощью await-ключевое слово, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="94882-540">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="94882-541">(Code Snippet - *GetAsync лаборатории - Ex04 - ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="94882-541">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="94882-542">В предыдущей версии, вы использовали **результат** свойства из **задачи** объекта, чтобы блокировать поток до возврата результата (синхронный).</span><span class="sxs-lookup"><span data-stu-id="94882-542">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="94882-543">Добавление **await** ключевое слово указывает компилятору для асинхронного ожидания для задачи, возвращаемой из вызова метода.</span><span class="sxs-lookup"><span data-stu-id="94882-543">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="94882-544">Это означает, что остальной части кода будет выполнен как обратный вызов только в том случае, после завершения работы метода ожидаемой.</span><span class="sxs-lookup"><span data-stu-id="94882-544">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="94882-545">Обратите внимание, что еще одну вещь: не нужно изменить в блок try-catch, чтобы выполнить эту операцию: исключения, которые происходят в фоновом режиме или в фоновом режиме по-прежнему будет перехвачено без дополнительных действий с помощью обработчика, предоставляемых платформой.</span><span class="sxs-lookup"><span data-stu-id="94882-545">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="94882-546">Изменить код, чтобы продолжить асинхронную реализацию, заменив строки с новым кодом, как показано ниже</span><span class="sxs-lookup"><span data-stu-id="94882-546">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="94882-547">(Code Snippet - *ReadAsStringAsync лаборатории - Ex04 - ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="94882-547">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="94882-548">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="94882-548">Run the application.</span></span> <span data-ttu-id="94882-549">Вы заметите существенных изменений, но код не будет блокировать поток из пула потоков, что лучше использования ресурсов сервера и повышая производительность.</span><span class="sxs-lookup"><span data-stu-id="94882-549">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-550">Дополнительные сведения о новой функции асинхронного программирования в лаборатории &quot; **асинхронное программирование в .NET 4.5 с помощью C# и Visual Basic** &quot; в состав набора обучения Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-550">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="94882-551">Задача 2 - обработка времени ожидания, токены отмены</span><span class="sxs-lookup"><span data-stu-id="94882-551">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="94882-552">Асинхронные методы действия, которые возвращают экземпляры задач также могут поддерживать значения времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="94882-552">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="94882-553">В этой задаче мы обновим код метода индекс для обработки времени ожидания сценария, с помощью токена отмены.</span><span class="sxs-lookup"><span data-stu-id="94882-553">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="94882-554">Вернитесь к Visual Studio и нажать клавишу **SHIFT + F5** остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="94882-554">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="94882-555">Добавьте следующий оператор using **HomeController.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="94882-555">Add the following using statement to the **HomeController.cs** file.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="94882-556">Обновление действия индекса для получения **CancellationToken** аргумент.</span><span class="sxs-lookup"><span data-stu-id="94882-556">Update the Index action to receive a **CancellationToken** argument.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="94882-557">Обновление **GetAsync** вызова для передачи токена отмены.</span><span class="sxs-lookup"><span data-stu-id="94882-557">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="94882-558">(Code Snippet - *SendAsync лаборатории - Ex04 - ASP.NET MVC 4 с CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="94882-558">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="94882-559">Decorate *индекс* метод с **AsyncTimeout** атрибуту присвоено значение 500 миллисекунд и **HandleError** атрибут, настроенный для обработки  **TaskCanceledException** путем перенаправления **TimedOut** представления.</span><span class="sxs-lookup"><span data-stu-id="94882-559">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="94882-560">(Code Snippet - *атрибуты лаборатории - Ex04 - ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="94882-560">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="94882-561">Откройте **PhotoController** класс и обновление **коллекции** метод задержки в миллисекундах выполнение 1000 (1 секунда), чтобы имитировать это продолжительная задача.</span><span class="sxs-lookup"><span data-stu-id="94882-561">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="94882-562">Откройте **Web.config** файл и включите настраиваемых ошибок, добавив следующий элемент.</span><span class="sxs-lookup"><span data-stu-id="94882-562">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="94882-563">Создание нового представления в **Views\Shared** с именем **TimedOut** и использование макета по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="94882-563">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="94882-564">В обозревателе решений щелкните правой кнопкой мыши **Views\Shared** папку и выберите **Add | Представление**.</span><span class="sxs-lookup"><span data-stu-id="94882-564">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="94882-565">![С помощью различных представлений для каждого мобильного устройства](whats-new-in-aspnet-mvc-4/_static/image36.png "с помощью различных представлений для каждого мобильного устройства")</span><span class="sxs-lookup"><span data-stu-id="94882-565">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="94882-566">*С помощью различных представлений для каждого мобильного устройства*</span><span class="sxs-lookup"><span data-stu-id="94882-566">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="94882-567">Обновление **TimedOut** Просмотр содержимого, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="94882-567">Update the **TimedOut** view content as shown below.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="94882-568">Запустите приложение и перейдите к корневой URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="94882-568">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="94882-569">Как вы добавили **Thread.Sleep** 1000 миллисекунд, вы получите ошибку времени ожидания, создаваемые **AsyncTimeout** атрибут и выявляйте с **HandleError** атрибут.</span><span class="sxs-lookup"><span data-stu-id="94882-569">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="94882-570">![Время ожидания исключения, обрабатываемого](whats-new-in-aspnet-mvc-4/_static/image37.png "исключение времени ожидания обработки")</span><span class="sxs-lookup"><span data-stu-id="94882-570">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="94882-571">*Исключение времени ожидания обработки*</span><span class="sxs-lookup"><span data-stu-id="94882-571">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="94882-572">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложение D: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="94882-572">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="94882-573">Сводка</span><span class="sxs-lookup"><span data-stu-id="94882-573">Summary</span></span>

<span data-ttu-id="94882-574">В этом практического занятия вы встречающейся некоторые из новых функций постоянно находятся в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="94882-574">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="94882-575">Подробно обсуждаются следующие понятия:</span><span class="sxs-lookup"><span data-stu-id="94882-575">The following concepts have been discussed:</span></span>

- <span data-ttu-id="94882-576">Воспользоваться преимуществами расширений для ASP.NET MVC проект шаблоны, в том числе нового шаблона проекта мобильного приложения</span><span class="sxs-lookup"><span data-stu-id="94882-576">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="94882-577">Использовать окна просмотра атрибут HTML5 и CSS медиа-запросов для улучшения отображения на мобильных устройствах</span><span class="sxs-lookup"><span data-stu-id="94882-577">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="94882-578">Использовать jQuery Mobile для прогрессивного улучшения, а также для создания оптимизированных для сенсорных экранов веб-Интерфейс</span><span class="sxs-lookup"><span data-stu-id="94882-578">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="94882-579">Создание представлений для мобильных устройств</span><span class="sxs-lookup"><span data-stu-id="94882-579">Create mobile-specific views</span></span>
- <span data-ttu-id="94882-580">Использовать компонент переключателя для переключения между представлениями мобильные и Классические приложения</span><span class="sxs-lookup"><span data-stu-id="94882-580">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="94882-581">Создание асинхронных контроллеров, используя поддержку задач</span><span class="sxs-lookup"><span data-stu-id="94882-581">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="94882-582">Приложение а. Использование фрагментов кода</span><span class="sxs-lookup"><span data-stu-id="94882-582">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="94882-583">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="94882-583">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="94882-584">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="94882-584">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="94882-585">![Фрагменты кода Visual Studio, чтобы вставить код в проект](whats-new-in-aspnet-mvc-4/_static/image38.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="94882-585">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="94882-586">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="94882-586">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="94882-587">***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="94882-587">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="94882-588">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="94882-588">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="94882-589">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="94882-589">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="94882-590">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="94882-590">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="94882-591">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="94882-591">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="94882-592">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="94882-592">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="94882-593">![Начните вводить имя фрагмента](whats-new-in-aspnet-mvc-4/_static/image39.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="94882-593">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="94882-594">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="94882-594">*Start typing the snippet name*</span></span>

<span data-ttu-id="94882-595">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](whats-new-in-aspnet-mvc-4/_static/image40.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="94882-595">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="94882-596">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="94882-596">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="94882-597">![Снова нажмите клавишу Tab и фрагмент будет расширяться](whats-new-in-aspnet-mvc-4/_static/image41.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="94882-597">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="94882-598">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="94882-598">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="94882-599">***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)***</span><span class="sxs-lookup"><span data-stu-id="94882-599">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="94882-600">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="94882-600">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="94882-601">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="94882-601">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="94882-602">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="94882-602">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="94882-603">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](whats-new-in-aspnet-mvc-4/_static/image42.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="94882-603">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="94882-604">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="94882-604">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="94882-605">![Выберите соответствующий фрагмент из списка, щелкнув по ней](whats-new-in-aspnet-mvc-4/_static/image43.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="94882-605">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="94882-606">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="94882-606">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="94882-607">Приложение б. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="94882-607">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="94882-608">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="94882-608">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="94882-609">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="94882-609">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="94882-610">Перейдите к [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="94882-610">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="94882-611">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-611">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="94882-612">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="94882-612">Click on **Install Now**.</span></span> <span data-ttu-id="94882-613">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="94882-613">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="94882-614">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="94882-614">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="94882-615">![Установка Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="94882-615">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="94882-616">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="94882-616">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="94882-617">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="94882-617">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="94882-619">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="94882-619">*Accepting the license terms*</span></span>
5. <span data-ttu-id="94882-620">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="94882-620">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="94882-622">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="94882-622">*Installation progress*</span></span>
6. <span data-ttu-id="94882-623">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="94882-623">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="94882-625">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="94882-625">*Installation completed*</span></span>
7. <span data-ttu-id="94882-626">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="94882-626">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="94882-627">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="94882-627">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="94882-629">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="94882-629">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="94882-630">Приложение c. Установка WebMatrix 2 и iPhone симулятор</span><span class="sxs-lookup"><span data-stu-id="94882-630">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="94882-631">Чтобы запустить свой сайт в iPhone на имитации устройства можно использовать расширения WebMatrix &quot;электрического симулятор Mobile для iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-631">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="94882-632">Кроме того можно настроить одного модуля для запуска симулятора из Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="94882-632">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="94882-633">Задача 1 - Установка WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="94882-633">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="94882-634">Перейдите к [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="94882-634">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="94882-635">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>WebMatrix 2</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="94882-635">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="94882-636">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="94882-636">Click on **Install Now**.</span></span> <span data-ttu-id="94882-637">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="94882-637">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="94882-638">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="94882-638">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="94882-639">![Установка WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Установка WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="94882-639">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="94882-640">*Установка WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="94882-640">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="94882-641">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="94882-641">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="94882-642">![Принятие условий лицензии на](whats-new-in-aspnet-mvc-4/_static/image50.png "принятие условий лицензии")</span><span class="sxs-lookup"><span data-stu-id="94882-642">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="94882-643">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="94882-643">*Accepting the license terms*</span></span>
5. <span data-ttu-id="94882-644">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="94882-644">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="94882-645">![Ход выполнения установки](whats-new-in-aspnet-mvc-4/_static/image51.png "ход выполнения установки")</span><span class="sxs-lookup"><span data-stu-id="94882-645">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="94882-646">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="94882-646">*Installation progress*</span></span>
6. <span data-ttu-id="94882-647">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="94882-647">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="94882-648">![Установка завершена](whats-new-in-aspnet-mvc-4/_static/image52.png "Установка завершена")</span><span class="sxs-lookup"><span data-stu-id="94882-648">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="94882-649">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="94882-649">*Installation completed*</span></span>
7. <span data-ttu-id="94882-650">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="94882-650">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="94882-651">Задача 2 - Установка расширения симулятора iPhone</span><span class="sxs-lookup"><span data-stu-id="94882-651">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="94882-652">Запустите **WebMatrix** и открыть любой существующий веб-сайт или создайте новую.</span><span class="sxs-lookup"><span data-stu-id="94882-652">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="94882-653">Нажмите кнопку **запуска** кнопки **Главная** ленты и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="94882-653">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="94882-654">![Добавление нового расширения WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Добавление нового расширения WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="94882-654">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="94882-655">*Добавление нового расширения WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="94882-655">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="94882-656">Выберите **iPhone симулятор** и нажмите кнопку **установить**.</span><span class="sxs-lookup"><span data-stu-id="94882-656">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="94882-657">![Просмотр расширения WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "расширения Обзор WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="94882-657">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="94882-658">*Просмотр расширения WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="94882-658">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="94882-659">Сведения о пакете, щелкните **установить** для продолжения установки расширения.</span><span class="sxs-lookup"><span data-stu-id="94882-659">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="94882-660">![расширение симулятора iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "расширение симулятора iPhone")</span><span class="sxs-lookup"><span data-stu-id="94882-660">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="94882-661">*расширение симулятора iPhone*</span><span class="sxs-lookup"><span data-stu-id="94882-661">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="94882-662">Прочитайте и примите лицензионное соглашение расширения.</span><span class="sxs-lookup"><span data-stu-id="94882-662">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="94882-663">![Расширения WebMatrix лицензионное соглашение](whats-new-in-aspnet-mvc-4/_static/image56.png "расширения WebMatrix лицензионное соглашение")</span><span class="sxs-lookup"><span data-stu-id="94882-663">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="94882-664">*Расширения WebMatrix лицензионное соглашение*</span><span class="sxs-lookup"><span data-stu-id="94882-664">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="94882-665">Теперь можно запустить веб-сайт из WebMatrix с помощью параметра симулятора iPhone.</span><span class="sxs-lookup"><span data-stu-id="94882-665">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="94882-666">![Запустить с помощью iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "запустить с помощью iPhone")</span><span class="sxs-lookup"><span data-stu-id="94882-666">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="94882-667">*Запустить с помощью iPhone*</span><span class="sxs-lookup"><span data-stu-id="94882-667">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="94882-668">Задача 3 - Настройка Visual Studio 2012 для запуска симулятора iPhone</span><span class="sxs-lookup"><span data-stu-id="94882-668">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="94882-669">Откройте **Visual Studio 2012** и откройте любой веб-сайт или создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="94882-669">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="94882-670">Щелкните стрелку рядом с и выберите **просмотр с помощью**.</span><span class="sxs-lookup"><span data-stu-id="94882-670">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="94882-671">![Просмотр с помощью](whats-new-in-aspnet-mvc-4/_static/image58.png "просмотр с помощью")</span><span class="sxs-lookup"><span data-stu-id="94882-671">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="94882-672">*Просмотр с помощью*</span><span class="sxs-lookup"><span data-stu-id="94882-672">*Browse with*</span></span>
3. <span data-ttu-id="94882-673">В &quot;просмотр с помощью&quot; диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="94882-673">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="94882-674">В &quot;добавить программу&quot; диалоговое окно, используйте следующие значения:</span><span class="sxs-lookup"><span data-stu-id="94882-674">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="94882-675"><strong>Программа</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (соответствующим образом обновить путь)</em></span><span class="sxs-lookup"><span data-stu-id="94882-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="94882-676">**Аргументы**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="94882-676">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="94882-677">**Понятное имя**: симулятора iPhone</span><span class="sxs-lookup"><span data-stu-id="94882-677">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="94882-678">![Добавление программы](whats-new-in-aspnet-mvc-4/_static/image59.png "Добавление программы")</span><span class="sxs-lookup"><span data-stu-id="94882-678">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="94882-679">*Добавьте программу, чтобы перейти с*</span><span class="sxs-lookup"><span data-stu-id="94882-679">*Add program to browse with*</span></span>
5. <span data-ttu-id="94882-680">Нажмите кнопку **ОК** и закрыть все диалоговые окна.</span><span class="sxs-lookup"><span data-stu-id="94882-680">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="94882-681">Теперь имеется возможность запуска веб-приложений в симуляторе iPhone из Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="94882-681">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="94882-682">![Просмотр с помощью iPhone симулятор](whats-new-in-aspnet-mvc-4/_static/image60.png "просмотр с помощью симулятора iPhone")</span><span class="sxs-lookup"><span data-stu-id="94882-682">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="94882-683">*Просмотр с помощью симулятора iPhone*</span><span class="sxs-lookup"><span data-stu-id="94882-683">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="94882-684">Приложение г. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="94882-684">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="94882-685">В этом приложении показано, как создать новый веб-сайт на портале управления Windows Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="94882-685">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="94882-686">Задача 1 - Создание нового веб-сайта с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="94882-686">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="94882-687">Перейдите к [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="94882-687">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-688">С помощью Windows Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика.</span><span class="sxs-lookup"><span data-stu-id="94882-688">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="94882-689">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="94882-689">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="94882-690">![Войдите на портал Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Войдите на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="94882-690">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="94882-691">*Войдите на портал управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="94882-691">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="94882-692">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="94882-692">Click **New** on the command bar.</span></span>

    <span data-ttu-id="94882-693">![Создание нового веб-сайта](whats-new-in-aspnet-mvc-4/_static/image62.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="94882-693">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="94882-694">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="94882-694">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="94882-695">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="94882-695">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="94882-696">Затем выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="94882-696">Then select **Quick Create** option.</span></span> <span data-ttu-id="94882-697">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="94882-697">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-698">Веб-сайта Windows Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="94882-698">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="94882-699">Быстрое создание позволяет развернуть завершенное веб-приложения для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="94882-699">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="94882-700">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="94882-700">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="94882-701">![Создание нового веб-сайта, с помощью быстрого создания](whats-new-in-aspnet-mvc-4/_static/image63.png "создания нового веб-сайта, с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="94882-701">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="94882-702">*Создание нового веб-сайта, с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="94882-702">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="94882-703">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="94882-703">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="94882-704">После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="94882-704">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="94882-705">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="94882-705">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="94882-706">![Выбрав новый веб-сайт](whats-new-in-aspnet-mvc-4/_static/image64.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="94882-706">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="94882-707">*Выбрав новый веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="94882-707">*Browsing to the new web site*</span></span>

    <span data-ttu-id="94882-708">![Веб-сайт работал](whats-new-in-aspnet-mvc-4/_static/image65.png "веб-узлом")</span><span class="sxs-lookup"><span data-stu-id="94882-708">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="94882-709">*Веб-сайт под управлением*</span><span class="sxs-lookup"><span data-stu-id="94882-709">*Web site running*</span></span>
6. <span data-ttu-id="94882-710">Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="94882-710">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="94882-711">![Открытие страницы управления веб-сайт](whats-new-in-aspnet-mvc-4/_static/image66.png "Открытие страницы управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="94882-711">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="94882-712">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="94882-712">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="94882-713">В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="94882-713">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-714">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайт Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="94882-714">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="94882-715">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации.</span><span class="sxs-lookup"><span data-stu-id="94882-715">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="94882-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений на веб-сайтах Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="94882-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="94882-717">![Профиль публикации веб-сайт загрузки](whats-new-in-aspnet-mvc-4/_static/image67.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="94882-717">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="94882-718">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="94882-718">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="94882-719">Скачайте файл профиля публикации в известном месте.</span><span class="sxs-lookup"><span data-stu-id="94882-719">Download the publish profile file to a known location.</span></span> <span data-ttu-id="94882-720">Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения на веб-сайтах, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94882-720">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="94882-721">![Сохранение файла профиля публикации](whats-new-in-aspnet-mvc-4/_static/image68.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="94882-721">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="94882-722">*Сохранение файла профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="94882-722">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="94882-723">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="94882-723">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="94882-724">Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="94882-724">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="94882-725">Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.</span><span class="sxs-lookup"><span data-stu-id="94882-725">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="94882-726">Вам потребуется сервер базы данных SQL для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="94882-726">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="94882-727">Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="94882-727">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="94882-728">Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="94882-728">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="94882-729">Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="94882-729">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="94882-730">Не создавайте базы данных, так как он будет создан в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="94882-730">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="94882-731">![Панель мониторинга базы данных SQL Server](whats-new-in-aspnet-mvc-4/_static/image69.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="94882-731">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="94882-732">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="94882-732">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="94882-733">В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="94882-733">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="94882-734">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="94882-734">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Добавление IP-адрес клиента](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="94882-736">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="94882-736">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="94882-737">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="94882-737">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="94882-739">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="94882-739">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="94882-740">Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="94882-740">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="94882-741">Вернитесь к решению для ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="94882-741">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="94882-742">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="94882-742">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="94882-743">![Публикация приложения](whats-new-in-aspnet-mvc-4/_static/image73.png "публикации приложения")</span><span class="sxs-lookup"><span data-stu-id="94882-743">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="94882-744">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="94882-744">*Publishing the web site*</span></span>
2. <span data-ttu-id="94882-745">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="94882-745">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="94882-746">![Импорт профиля публикации](whats-new-in-aspnet-mvc-4/_static/image74.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="94882-746">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="94882-747">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="94882-747">*Importing publish profile*</span></span>
3. <span data-ttu-id="94882-748">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="94882-748">Click **Validate Connection**.</span></span> <span data-ttu-id="94882-749">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="94882-749">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94882-750">Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.</span><span class="sxs-lookup"><span data-stu-id="94882-750">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="94882-751">![Проверка подключения](whats-new-in-aspnet-mvc-4/_static/image75.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="94882-751">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="94882-752">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="94882-752">*Validating connection*</span></span>
4. <span data-ttu-id="94882-753">В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="94882-753">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="94882-754">![Конфигурация веб-развертывания](whats-new-in-aspnet-mvc-4/_static/image76.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="94882-754">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="94882-755">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="94882-755">*Web deploy configuration*</span></span>
5. <span data-ttu-id="94882-756">Настройте подключение к базе данных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="94882-756">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="94882-757">В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="94882-757">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="94882-758">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="94882-758">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="94882-759">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="94882-759">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="94882-760">Введите новое имя базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="94882-760">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="94882-761">![Настройка строки подключения назначения](whats-new-in-aspnet-mvc-4/_static/image77.png "Настройка строка соединения с назначением")</span><span class="sxs-lookup"><span data-stu-id="94882-761">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="94882-762">*Настройка строки подключения назначения*</span><span class="sxs-lookup"><span data-stu-id="94882-762">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="94882-763">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="94882-763">Then click **OK**.</span></span> <span data-ttu-id="94882-764">При появлении запроса на создание базы данных нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="94882-764">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="94882-765">![Создание базы данных](whats-new-in-aspnet-mvc-4/_static/image78.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="94882-765">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="94882-766">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="94882-766">*Creating the database*</span></span>
7. <span data-ttu-id="94882-767">В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="94882-767">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="94882-768">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="94882-768">Then click **Next**.</span></span>

    <span data-ttu-id="94882-769">![Строка подключения, указывающий на базу данных SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "строку подключения, указывающий на базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="94882-769">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="94882-770">*Строка подключения, указывающий на базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="94882-770">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="94882-771">В **предварительной версии** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="94882-771">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="94882-772">![Публикация веб-приложения](whats-new-in-aspnet-mvc-4/_static/image80.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="94882-772">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="94882-773">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="94882-773">*Publishing the web application*</span></span>
9. <span data-ttu-id="94882-774">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="94882-774">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="94882-775">![Приложение опубликовано в Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="94882-775">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="94882-776">*Приложение будет публиковаться в Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="94882-776">*Application published to Windows Azure*</span></span>
