---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Создайте новый проект ASP.NET MVC | Документация Майкрософт
author: microsoft
description: Шаг 1 показано, как поместить базовая структура приложение NerdDinner на месте.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374280"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="6324b-103">Создайте новый проект ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6324b-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="6324b-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6324b-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="6324b-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="6324b-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="6324b-106">Это этап 1 из бесплатной [руководство по использованию приложения «NerdDinner»](introducing-the-nerddinner-tutorial.md) , пошаговое рассмотрение как создать небольшой, но завершить, веб-приложения с помощью ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="6324b-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="6324b-107">Шаг 1 показано, как поместить базовая структура приложение NerdDinner на месте.</span><span class="sxs-lookup"><span data-stu-id="6324b-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="6324b-108">Если вы используете ASP.NET MVC 3, рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="6324b-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="6324b-109">NerdDinner шаг 1: Файл -&gt;нового проекта</span><span class="sxs-lookup"><span data-stu-id="6324b-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="6324b-110">Начнем наше приложение NerdDinner, выбрав **файл -&gt;новый проект** элемента меню в Visual Studio 2008 или бесплатный Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="6324b-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="6324b-111">Откроется диалоговое окно «Новый проект».</span><span class="sxs-lookup"><span data-stu-id="6324b-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="6324b-112">Чтобы создать новое приложение ASP.NET MVC, мы выберите узел «Web» в левой части диалогового окна и затем выберите шаблон проекта «Веб-приложение ASP.NET MVC» в правой части:</span><span class="sxs-lookup"><span data-stu-id="6324b-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="6324b-113">*Важно: Убедитесь, что вы загрузили и установить ASP.NET MVC — в противном случае оно не будет отображаться в диалоговом окне нового проекта. Можно использовать версии 2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) Если вы еще не установили еще (ASP.NET MVC можно использовать в «платформы веб -&gt;платформ и сред выполнения» раздела).*</span><span class="sxs-lookup"><span data-stu-id="6324b-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="6324b-114">Мы будем называть новый проект, который мы собираемся создать «NerdDinner» и нажмите кнопку «ОК», чтобы создать его.</span><span class="sxs-lookup"><span data-stu-id="6324b-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="6324b-115">После нажатия кнопки «ОК» Visual Studio приведет к появлению дополнительных диалогового окна, который запрашивает при необходимости создавать проект модульного теста для нового приложения, а также.</span><span class="sxs-lookup"><span data-stu-id="6324b-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="6324b-116">Этот проект модульного теста позволяет нам создавать автоматические тесты, проверяющие функциональность и поведение нашего приложения (что-то мы обсудим как задача далее в этом руководстве).</span><span class="sxs-lookup"><span data-stu-id="6324b-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="6324b-117">Раскрывающийся список «Платформы тестирования» в диалоговом окне выше заполняется все доступные ASP.NET MVC шаблоны проекта модульных тестов установлен на компьютере.</span><span class="sxs-lookup"><span data-stu-id="6324b-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="6324b-118">Версии, можно загрузить для XUnit, NUnit и MBUnit.</span><span class="sxs-lookup"><span data-stu-id="6324b-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="6324b-119">Также поддерживается встроенная платформа модульного тестирования.</span><span class="sxs-lookup"><span data-stu-id="6324b-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="6324b-120">*Примечание: Visual Studio модульного тестирования доступен только с помощью Visual Studio 2008 Professional и более поздних версиях. Если вы используете VS 2008 Standard Edition или Visual Web Developer 2008 Express необходимо будет загрузить и установить расширения NUnit и MBUnit XUnit для ASP.NET MVC в порядке для этого диалогового окна для отображения. В диалоговом окне не отобразится, если нет всех платформ тестирования, которые установлены.*</span><span class="sxs-lookup"><span data-stu-id="6324b-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="6324b-121">Мы используйте имя по умолчанию «NerdDinner.Tests» для тестового проекта, которые будут созданы и используйте параметр «Модульных тестов Visual Studio» framework.</span><span class="sxs-lookup"><span data-stu-id="6324b-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="6324b-122">При нажатии кнопки «ОК» Visual Studio создаст решение с двумя проектами, в нем — один для нашего веб-приложения и один для наших модульных тестов:</span><span class="sxs-lookup"><span data-stu-id="6324b-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="6324b-123">Изучение структуры каталогов NerdDinner</span><span class="sxs-lookup"><span data-stu-id="6324b-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="6324b-124">При создании нового приложения ASP.NET MVC с помощью Visual Studio автоматически добавляет число файлов и каталогов в проект:</span><span class="sxs-lookup"><span data-stu-id="6324b-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="6324b-125">Проекты ASP.NET MVC по умолчанию иметь шесть каталоги верхнего уровня:</span><span class="sxs-lookup"><span data-stu-id="6324b-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="6324b-126">**Каталог**</span><span class="sxs-lookup"><span data-stu-id="6324b-126">**Directory**</span></span> | <span data-ttu-id="6324b-127">**Назначение**</span><span class="sxs-lookup"><span data-stu-id="6324b-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="6324b-128">**/ Контроллеров**</span><span class="sxs-lookup"><span data-stu-id="6324b-128">**/Controllers**</span></span> | <span data-ttu-id="6324b-129">Место расположения классы контроллеров, которые обрабатывают запросы на URL-адрес</span><span class="sxs-lookup"><span data-stu-id="6324b-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="6324b-130">**/ Моделей**</span><span class="sxs-lookup"><span data-stu-id="6324b-130">**/Models**</span></span> | <span data-ttu-id="6324b-131">Место расположения классы, представляющие и манипулировать данными</span><span class="sxs-lookup"><span data-stu-id="6324b-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="6324b-132">**И представлений**</span><span class="sxs-lookup"><span data-stu-id="6324b-132">**/Views**</span></span> | <span data-ttu-id="6324b-133">Место расположения файлов шаблонов пользовательского интерфейса, которые отвечают за подготовкой к просмотру</span><span class="sxs-lookup"><span data-stu-id="6324b-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="6324b-134">**/ Scripts**</span><span class="sxs-lookup"><span data-stu-id="6324b-134">**/Scripts**</span></span> | <span data-ttu-id="6324b-135">Места размещения файлов библиотек JavaScript и скрипты (.js)</span><span class="sxs-lookup"><span data-stu-id="6324b-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="6324b-136">**/ Content**</span><span class="sxs-lookup"><span data-stu-id="6324b-136">**/Content**</span></span> | <span data-ttu-id="6324b-137">Размещения CSS и файлы изображений и другого содержимого не для динамической и не JavaScript</span><span class="sxs-lookup"><span data-stu-id="6324b-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="6324b-138">**/ App\_данных**</span><span class="sxs-lookup"><span data-stu-id="6324b-138">**/App\_Data**</span></span> | <span data-ttu-id="6324b-139">Там, где хранятся файлы данных необходимо чтение и запись.</span><span class="sxs-lookup"><span data-stu-id="6324b-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="6324b-140">ASP.NET MVC не требуется эта структура.</span><span class="sxs-lookup"><span data-stu-id="6324b-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="6324b-141">На самом деле, разработчики, работающие на большие приложения обычно секционирует приложение вверх по нескольким проектам, чтобы сделать его более управляемым (например: классы модели данных часто работают в отдельном проекте библиотеки классов из веб-приложения).</span><span class="sxs-lookup"><span data-stu-id="6324b-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="6324b-142">Структура проекта по умолчанию, однако предоставляет соглашение об directory неплохо по умолчанию, который можно использовать для сохранения чистого наши проблемы приложения.</span><span class="sxs-lookup"><span data-stu-id="6324b-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="6324b-143">Если планируется расширить каталог /Controllers очень скоро найдется что Visual Studio добавляет два класса контроллера — HomeController и AccountController — по умолчанию в проект:</span><span class="sxs-lookup"><span data-stu-id="6324b-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="6324b-144">Если планируется расширить каталог/Views, очень скоро найдется три вложенных каталогов — / Home, / Account и/Shared под —, а также несколько файлов в них также были добавлены в проект по умолчанию шаблон:</span><span class="sxs-lookup"><span data-stu-id="6324b-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="6324b-145">Если планируется расширить, контента и каталоги/Scripts, очень скоро найдется, файл Site.css, используемого для стиля HTML на сайте, а также библиотеки JavaScript, ASP.NET AJAX и jQuery можно включить поддержку в приложении:</span><span class="sxs-lookup"><span data-stu-id="6324b-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="6324b-146">Когда мы разверните проект NerdDinner.Tests очень скоро найдется двух классов, содержащих модульные тесты для наших классов контроллера:</span><span class="sxs-lookup"><span data-stu-id="6324b-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="6324b-147">Эти файлы по умолчанию, добавленные в Visual Studio предоставляют нам с простой структурой работающего приложения — с домашней страницы, страницы, страниц входа/выхода/регистрации учетной записи и страницей необработанная ошибка (все привязав и работает без дополнительной настройки).</span><span class="sxs-lookup"><span data-stu-id="6324b-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="6324b-148">Запущено приложение NerdDinner</span><span class="sxs-lookup"><span data-stu-id="6324b-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="6324b-149">Мы можно запустить проект, выбрав либо **отладки -&gt;начать отладку** или **отладки -&gt;Запуск без отладки** пункты меню:</span><span class="sxs-lookup"><span data-stu-id="6324b-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="6324b-150">Это запуска встроенного веб сервера ASP.NET, входящий в состав Visual Studio и запустить наше приложение:</span><span class="sxs-lookup"><span data-stu-id="6324b-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="6324b-151">Ниже приведен на домашнюю страницу для нашего проекта (URL-адрес: «/») при его запуске:</span><span class="sxs-lookup"><span data-stu-id="6324b-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="6324b-152">Щелкнув вкладку «О программе» отображается страница (URL-адрес: «/ Home/About»):</span><span class="sxs-lookup"><span data-stu-id="6324b-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="6324b-153">Щелкнув ссылку «Вход» в правом верхнем углу проводит для нас на страницу входа (URL-адрес: «/ учетной записи и входа в систему»)</span><span class="sxs-lookup"><span data-stu-id="6324b-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="6324b-154">Если у нас учетной записи входа, мы можно щелкнуть ссылку register (URL-адрес: «/ учетная запись/Register») для создания:</span><span class="sxs-lookup"><span data-stu-id="6324b-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="6324b-155">Код для реализации выше дома и выход из системы / register предусмотрена по умолчанию при создании нашего проекта.</span><span class="sxs-lookup"><span data-stu-id="6324b-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="6324b-156">Мы будем использовать как начальную точку нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="6324b-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="6324b-157">Тестирование приложения NerdDinner</span><span class="sxs-lookup"><span data-stu-id="6324b-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="6324b-158">Если мы используем Professional Edition или более поздней версии Visual Studio 2008, встроенная поддержка модульного тестирования интегрированной среды разработки в Visual Studio можно использовать для тестирования проекта:</span><span class="sxs-lookup"><span data-stu-id="6324b-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="6324b-159">Выбрав один из перечисленных выше способов открыть панель «Результаты теста» в интегрированной среде разработки и предоставить нам со статусом успех или сбой на 27 модульных тестах, включенных в нашем новом проекте, которые охватывают встроенные функции:</span><span class="sxs-lookup"><span data-stu-id="6324b-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="6324b-160">Далее в этом руководстве мы поговорим о автоматизированного тестирования и добавить дополнительные модульные тесты, которые покрывают функциональные возможности приложения, которые мы реализуем.</span><span class="sxs-lookup"><span data-stu-id="6324b-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="6324b-161">Следующий шаг</span><span class="sxs-lookup"><span data-stu-id="6324b-161">Next Step</span></span>

<span data-ttu-id="6324b-162">Теперь вы найдете структуру базового приложения на месте.</span><span class="sxs-lookup"><span data-stu-id="6324b-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="6324b-163">Давайте теперь [создать базу данных для хранения наших данных приложения](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="6324b-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6324b-164">[Назад](introducing-the-nerddinner-tutorial.md)
> [Вперед](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="6324b-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
