---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: "Создайте новый проект ASP.NET MVC | Документы Microsoft"
author: microsoft
description: "Шаг 1 показано, как поместить базовую структуру приложения обновление NerdDinner на месте."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="b8574-103">Создайте новый проект ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b8574-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="b8574-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b8574-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b8574-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="b8574-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="b8574-106">Это 1 из бесплатной [учебника «Обновление NerdDinner» приложения](introducing-the-nerddinner-tutorial.md) , обходов сквозной как построить небольшой, но завершается веб-приложения с помощью ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="b8574-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="b8574-107">Шаг 1 показано, как поместить базовую структуру приложения обновление NerdDinner на месте.</span><span class="sxs-lookup"><span data-stu-id="b8574-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="b8574-108">При использовании ASP.NET MVC 3, которому рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="b8574-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="b8574-109">Обновление NerdDinner шаг 1: Файл -&gt;нового проекта</span><span class="sxs-lookup"><span data-stu-id="b8574-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="b8574-110">Начнем обновление NerdDinner приложения, выбрав **файл -&gt;новый проект** элемента меню в Visual Studio 2008 или произвольным Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="b8574-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="b8574-111">Откроется диалоговое окно «Создание проекта».</span><span class="sxs-lookup"><span data-stu-id="b8574-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="b8574-112">Создание нового приложения ASP.NET MVC, мы будет выберите узел «Web» в левой части диалогового окна и выберите шаблон проекта «Веб-приложение ASP.NET MVC» справа:</span><span class="sxs-lookup"><span data-stu-id="b8574-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="b8574-113">*Внимание: Убедитесь, что вы загрузили и установили ASP.NET MVC — в противном случае он не будет отображаться в диалоговом окне нового проекта. Можно использовать V2 из [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) , если он еще не установлен еще (ASP.NET MVC можно использовать в «веб-платформы -&gt;платформы и среды выполнения» раздела).*</span><span class="sxs-lookup"><span data-stu-id="b8574-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="b8574-114">Мы назовем новый проект, который мы собираемся создавать «Обновление NerdDinner» и нажмите кнопку «ОК», чтобы создать его.</span><span class="sxs-lookup"><span data-stu-id="b8574-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="b8574-115">После нажатия кнопки «ОК» Visual Studio откроет дополнительных диалоговое окно, которое запрашивает у нас Создание проекта модульного теста для нового приложения, а также при необходимости.</span><span class="sxs-lookup"><span data-stu-id="b8574-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="b8574-116">Этот проект модульных тестов позволяет создавать автоматические тесты, проверяющие функциональность и поведение приложения (что-то, мы обсудим как задача далее в этом учебнике).</span><span class="sxs-lookup"><span data-stu-id="b8574-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="b8574-117">Раскрывающийся список «Платформы тестирования» в диалоговом окне выше заполняется все доступные ASP.NET MVC шаблоны модульных тестов проекта установлен на компьютере.</span><span class="sxs-lookup"><span data-stu-id="b8574-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="b8574-118">Версии можно загрузить для NUnit, MBUnit и XUnit.</span><span class="sxs-lookup"><span data-stu-id="b8574-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="b8574-119">Также поддерживается встроенная платформа модульного тестирования.</span><span class="sxs-lookup"><span data-stu-id="b8574-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="b8574-120">*Примечание: Visual Studio модульного тестирования доступна только при наличии Visual Studio 2008 Professional и более поздних версий. Если вы используете VS 2008 Standard Edition или Visual Web Developer 2008 Express потребуется загрузить и установить расширения NUnit, MBUnit или XUnit для ASP.NET MVC в порядке для этого диалога, который будет отображаться. В диалоговом окне не отобразится, если нет ни одной из платформ тестирования установлены.*</span><span class="sxs-lookup"><span data-stu-id="b8574-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="b8574-121">Используется имя «NerdDinner.Tests» по умолчанию для тестового проекта, который создается и использовать параметр «Модульных тестов Visual Studio» платформы.</span><span class="sxs-lookup"><span data-stu-id="b8574-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="b8574-122">После нажатия кнопки кнопки «ОК» Visual Studio создаст решение с двумя проектами, в нем — один для наших веб-приложение и один для наших модульных тестов:</span><span class="sxs-lookup"><span data-stu-id="b8574-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="b8574-123">Изучение структуры каталогов обновление NerdDinner</span><span class="sxs-lookup"><span data-stu-id="b8574-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="b8574-124">При создании нового приложения ASP.NET MVC с помощью Visual Studio автоматически добавляет число файлов и каталогов в проект:</span><span class="sxs-lookup"><span data-stu-id="b8574-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="b8574-125">Проекты ASP.NET MVC по умолчанию имеют шесть каталоги верхнего уровня:</span><span class="sxs-lookup"><span data-stu-id="b8574-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="b8574-126">**Каталог**</span><span class="sxs-lookup"><span data-stu-id="b8574-126">**Directory**</span></span> | <span data-ttu-id="b8574-127">**Назначение**</span><span class="sxs-lookup"><span data-stu-id="b8574-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="b8574-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="b8574-128">**/Controllers**</span></span> | <span data-ttu-id="b8574-129">Где размещен контроллер классах, обрабатывающих запросы на URL-адрес</span><span class="sxs-lookup"><span data-stu-id="b8574-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="b8574-130">**И моделей**</span><span class="sxs-lookup"><span data-stu-id="b8574-130">**/Models**</span></span> | <span data-ttu-id="b8574-131">Размещения классы, представляющие и работы с данными</span><span class="sxs-lookup"><span data-stu-id="b8574-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="b8574-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="b8574-132">**/Views**</span></span> | <span data-ttu-id="b8574-133">Места размещения файлов шаблона пользовательского интерфейса, отвечающих за формат подготовки к просмотру</span><span class="sxs-lookup"><span data-stu-id="b8574-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="b8574-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="b8574-134">**/Scripts**</span></span> | <span data-ttu-id="b8574-135">Места размещения файлов библиотек JavaScript и скрипты (.js)</span><span class="sxs-lookup"><span data-stu-id="b8574-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="b8574-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="b8574-136">**/Content**</span></span> | <span data-ttu-id="b8574-137">Размещения CSS и файлы изображений и другого содержимого не для динамической/не JavaScript</span><span class="sxs-lookup"><span data-stu-id="b8574-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="b8574-138">**/ Приложения\_данных**</span><span class="sxs-lookup"><span data-stu-id="b8574-138">**/App\_Data**</span></span> | <span data-ttu-id="b8574-139">Где хранятся файлы данных требуется для чтения и записи.</span><span class="sxs-lookup"><span data-stu-id="b8574-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="b8574-140">Эта структура ASP.NET MVC не требуется.</span><span class="sxs-lookup"><span data-stu-id="b8574-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="b8574-141">На самом деле, разработчиков, работающих в больших приложениях обычно разделения приложения вверх по нескольким проектам, чтобы сделать его более управляемым (например: классы модели данных часто идут в отдельном проекте библиотеки классов из веб-приложения).</span><span class="sxs-lookup"><span data-stu-id="b8574-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="b8574-142">Структура проекта по умолчанию, тем не менее, корпорация Майкрософт предоставляет соглашение каталог работы с низким приоритетом по умолчанию, который можно использовать для сохранения чистой наши проблемы приложения.</span><span class="sxs-lookup"><span data-stu-id="b8574-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="b8574-143">Когда мы откройте каталог /Controllers мы увидите, что Visual Studio два класса контроллера — HomeController и AccountController — по умолчанию добавляется в проект:</span><span class="sxs-lookup"><span data-stu-id="b8574-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="b8574-144">Когда мы откройте каталог /Views, мы найдете три вложенные каталоги — / Home, / Account и/Shared —, а также несколько файлов в них также были добавлены в проект по умолчанию шаблон:</span><span class="sxs-lookup"><span data-stu-id="b8574-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="b8574-145">Когда мы развернуть /Content и каталоги/Scripts, мы найти поддержку файл Site.css, используемого для стиля HTML на веб-узле, а также библиотеки JavaScript, которые можно включить ASP.NET AJAX и jQuery в приложении:</span><span class="sxs-lookup"><span data-stu-id="b8574-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="b8574-146">Когда мы разверните проект NerdDinner.Tests мы найти двух классов, содержащих модульные тесты для наших классов контроллера:</span><span class="sxs-lookup"><span data-stu-id="b8574-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="b8574-147">Эти файлы по умолчанию, добавленные в Visual Studio предоставляют нам с простой структурой в работающем приложении — с домашней страницы, о страницы, страницы входа и выхода из системы или регистрации учетной записи и необработанных ошибок (всех проводного доступа и работает без дополнительной настройки).</span><span class="sxs-lookup"><span data-stu-id="b8574-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="b8574-148">Запуск приложения обновление NerdDinner</span><span class="sxs-lookup"><span data-stu-id="b8574-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="b8574-149">Мы можно запустить проект, либо выбрав **отладки -&gt;начать отладку** или **отладки -&gt;Запуск без отладки** пункты меню:</span><span class="sxs-lookup"><span data-stu-id="b8574-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="b8574-150">Это будет запустите встроенного веб сервера ASP.NET, входящий в состав Visual Studio и выполните наше приложение:</span><span class="sxs-lookup"><span data-stu-id="b8574-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="b8574-151">Ниже приведен домашней страницы для нашего нового проекта (URL-адрес: «/») при запуске:</span><span class="sxs-lookup"><span data-stu-id="b8574-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="b8574-152">На вкладке «Азот» отображает о странице (URL-адрес: «/ Home или»):</span><span class="sxs-lookup"><span data-stu-id="b8574-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="b8574-153">Щелкнув ссылку «Вход» в правом верхнем отсылает страницу входа в систему (URL-адрес: «/ Account/входа в систему»)</span><span class="sxs-lookup"><span data-stu-id="b8574-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="b8574-154">Если у нас нет учетной записи входа, мы можно щелкнуть ссылку регистра (URL-адрес: «/ Account/Register») для ее создания:</span><span class="sxs-lookup"><span data-stu-id="b8574-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="b8574-155">Код для реализации выше дома и выхода из системы / register предусмотрена по умолчанию когда мы создавали наш новый проект.</span><span class="sxs-lookup"><span data-stu-id="b8574-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="b8574-156">Мы будем использовать в качестве точки запуска нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="b8574-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="b8574-157">Тестирование приложения обновление NerdDinner</span><span class="sxs-lookup"><span data-stu-id="b8574-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="b8574-158">Если мы используем Professional Edition или более поздней версии Visual Studio 2008, встроенной поддержки модульных тестов интегрированной среды разработки в Visual Studio можно использовать для тестирования проекта:</span><span class="sxs-lookup"><span data-stu-id="b8574-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="b8574-159">Выбрав один из перечисленных выше параметров будет открыть панель «Результаты» в Интегрированной среде разработки, а также получить со статусом успех или сбой на 27 модульных тестах, включенных в новый проект, охватывающие их встроенные функциональные возможности:</span><span class="sxs-lookup"><span data-stu-id="b8574-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="b8574-160">Далее в этом учебнике мы будем Подробнее об автоматических тестов и добавить дополнительные модульные тесты, которые покрывают функциональные возможности приложений, которые мы реализовали.</span><span class="sxs-lookup"><span data-stu-id="b8574-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="b8574-161">Следующий шаг</span><span class="sxs-lookup"><span data-stu-id="b8574-161">Next Step</span></span>

<span data-ttu-id="b8574-162">Теперь имеется структура простого приложения на месте.</span><span class="sxs-lookup"><span data-stu-id="b8574-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="b8574-163">Давайте теперь [создать базу данных для хранения данных приложения](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="b8574-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b8574-164">[Назад](introducing-the-nerddinner-tutorial.md)
[Вперед](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="b8574-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
