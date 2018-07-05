---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Создание API-интерфейсов RESTful с помощью веб-API ASP.NET | Документация Майкрософт
author: rick-anderson
description: В последние годы стало clear, что HTTP не только для подготовки HTML-страницы. Это также мощную платформу для построения веб-API, с помощью o несколько...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363524"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="1fcfc-104">Создание API-интерфейсов RESTful с помощью веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1fcfc-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="1fcfc-105">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="1fcfc-106">В последние годы стало clear, что HTTP не только для подготовки HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="1fcfc-107">Это также мощную платформу для построения веб-API, такие как с помощью несколько команд (GET, POST и т. д.) плюс несколько простых концепции *идентификаторы URI* и *заголовки*.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="1fcfc-108">Веб-API ASP.NET — это набор компонентов, которые упрощают программирование HTTP.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="1fcfc-109">Так как она построена на основе среды выполнения ASP.NET MVC, веб-API автоматически обрабатывает сведения низкого уровня транспорта протокола HTTP.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="1fcfc-110">В то же время веб-API, естественным образом, предоставляет модель программирования HTTP.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="1fcfc-111">На самом деле, одна из целей веб-API — *не* абстрагировании от реальности HTTP.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="1fcfc-112">Таким образом веб-API, гибкие и легко расширяется.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="1fcfc-113">В этой практической работу вы воспользуетесь веб-API для создания простой REST API для приложения диспетчера контактов.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="1fcfc-114">Вы также создадите клиента для использования API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="1fcfc-115">Архитектурный стиль REST оказалось эффективным способом использовать HTTP -, несмотря на то, что это, конечно, единственным допустимым подход к HTTP.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="1fcfc-116">Диспетчер контактов будет предоставлять RESTful для списка, добавление и удаление контактов, среди прочего.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="1fcfc-117">Это лабораторное занятие требует понимания HTTP, REST и предполагается, что вы имеете базовые знания рабочий HTML, JavaScript и jQuery.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="1fcfc-118">Веб-узла ASP.NET имеет выделенный в веб-API ASP.NET Framework в область [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="1fcfc-119">Этот сайт будет продолжать последние сведения, примеры и новостей, относящихся к веб-API, поэтому она регулярно проверяйте Если вы хотите более подробно рассмотрим искусством создания пользовательского веб-API, доступных для framework практически с любого устройства или разработки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="1fcfc-120">Веб-API, аналогичную ASP.NET MVC 4 ASP.NET имеет большую гибкость с точки зрения отделение слой служб от контроллеров, позволяя использовать несколько доступных платформ внедрения зависимостей довольно просто.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="1fcfc-121">В MSDN, показывающий, как использовать Ninject для внедрения зависимостей в проекте веб-API ASP.NET, его можно загрузить из есть хороший пример [здесь](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="1fcfc-122">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных в [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1fcfc-123">Цели</span><span class="sxs-lookup"><span data-stu-id="1fcfc-123">Objectives</span></span>

<span data-ttu-id="1fcfc-124">В этой практической работу вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="1fcfc-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="1fcfc-125">Реализации веб-API RESTful</span><span class="sxs-lookup"><span data-stu-id="1fcfc-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="1fcfc-126">Для вызова API из HTML-клиента</span><span class="sxs-lookup"><span data-stu-id="1fcfc-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1fcfc-127">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1fcfc-127">Prerequisites</span></span>

<span data-ttu-id="1fcfc-128">Для завершения этой практической работу требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="1fcfc-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="1fcfc-129">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложении Б](#AppendixB) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1fcfc-130">Установка</span><span class="sxs-lookup"><span data-stu-id="1fcfc-130">Setup</span></span>

<span data-ttu-id="1fcfc-131">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="1fcfc-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="1fcfc-132">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="1fcfc-133">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="1fcfc-134">Если вы не знакомы с фрагменты кода Visual Studio и хотите научиться использовать их, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложении A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1fcfc-135">Упражнения</span><span class="sxs-lookup"><span data-stu-id="1fcfc-135">Exercises</span></span>

<span data-ttu-id="1fcfc-136">Это Практическое занятие включает в себя следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="1fcfc-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="1fcfc-137">Упражнение 1: Создание только для чтения веб-API</span><span class="sxs-lookup"><span data-stu-id="1fcfc-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="1fcfc-138">Упражнение 2: Создание, чтение и запись веб-API</span><span class="sxs-lookup"><span data-stu-id="1fcfc-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="1fcfc-139">Упражнение 3: Использование веб-API из HTML-клиента</span><span class="sxs-lookup"><span data-stu-id="1fcfc-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="1fcfc-140">Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="1fcfc-141">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="1fcfc-142">Предполагаемое время для выполнения этого лабораторного: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="1fcfc-143">Упражнение 1: Создание только для чтения веб-API</span><span class="sxs-lookup"><span data-stu-id="1fcfc-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="1fcfc-144">В этом упражнении вы реализуете методы GET только для чтения для диспетчера контактов.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="1fcfc-145">Задача 1 - Создание проекта API</span><span class="sxs-lookup"><span data-stu-id="1fcfc-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="1fcfc-146">В этой задаче будет использовать новые шаблоны веб-проектов ASP.NET для создания веб-приложения веб-API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="1fcfc-147">Запустите **Visual Studio 2012 Express для Web**, для этого перейдите к **запустить** и тип **VS Express для Web** нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="1fcfc-148">Из **файл** меню, выберите **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="1fcfc-149">Выберите **Visual C# | Web** тип в представлении дерева проекта типа проекта, а затем выберите **веб-приложение ASP.NET MVC 4** тип проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="1fcfc-150">Установите проект **имя** для *ContactManager* и **имя решения** для *начать*, нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="1fcfc-151">![Создание нового ASP.NET MVC 4.0 проект веб-приложения](build-restful-apis-with-aspnet-web-api/_static/image1.png "Создание нового ASP.NET MVC 4.0 проект веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="1fcfc-152">*Создание нового ASP.NET MVC 4.0 проект веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="1fcfc-153">В диалоговом окне типа проекта ASP.NET MVC 4, выберите **веб-API** тип проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="1fcfc-154">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-154">Click **OK**.</span></span>

    <span data-ttu-id="1fcfc-155">![Указывает тип проекта веб-API](build-restful-apis-with-aspnet-web-api/_static/image2.png "указывает тип проекта веб-API")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="1fcfc-156">*Указывает тип проекта веб-API*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="1fcfc-157">Задача 2 - Создание контроллеров API диспетчера контактов</span><span class="sxs-lookup"><span data-stu-id="1fcfc-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="1fcfc-158">В этой задаче вы создадите классы контроллера, в которых будет находиться методов API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="1fcfc-159">Удалите файл с именем **ValuesController.cs** в **контроллеров** папку из проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="1fcfc-160">Щелкните правой кнопкой мыши **контроллеров** папки в проект и выберите **Add | Контроллер** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="1fcfc-161">![Добавление в проект новый контроллер](build-restful-apis-with-aspnet-web-api/_static/image3.png "добавлении нового контроллера в проект")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="1fcfc-162">*Добавление в проект новый контроллер*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="1fcfc-163">В **Добавление контроллера** появившемся диалоговом окне выберите **пустой контроллер API** меню шаблон.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="1fcfc-164">Имя класса контроллера **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="1fcfc-165">Щелкните **Add.**</span><span class="sxs-lookup"><span data-stu-id="1fcfc-165">Then, click **Add.**</span></span>

    <span data-ttu-id="1fcfc-166">![Используя диалоговое окно добавления контроллера для создания нового контроллера веб-API](build-restful-apis-with-aspnet-web-api/_static/image4.png "используя диалоговое окно добавления контроллера для создания нового контроллера веб-API")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="1fcfc-167">*Используя диалоговое окно добавления контроллера для создания нового контроллера веб-API*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="1fcfc-168">Добавьте следующий код, чтобы **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="1fcfc-169">(Code Snippet - *метод API Get лаборатории API веб - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="1fcfc-170">Нажмите клавишу **F5** для отладки приложения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="1fcfc-171">Откроется домашняя страница по умолчанию для проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="1fcfc-172">![Домашняя страница по умолчанию приложения веб-API ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Домашняя страница по умолчанию приложения веб-API ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="1fcfc-173">*Домашняя страница по умолчанию приложения веб-API ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="1fcfc-174">В окне Internet Explorer, нажмите клавишу **F12** клавишу, чтобы открыть **средств разработчика** окна.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="1fcfc-175">Нажмите кнопку **сети** , а затем щелкните **начать захват** кнопку, чтобы начать отслеживать сетевой трафик в окно.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="1fcfc-176">![Открыв вкладку "Сеть" и затем запускать сбор по сети](build-restful-apis-with-aspnet-web-api/_static/image6.png "открыв вкладку \"Сеть\" и затем запускать сбор по сети")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="1fcfc-177">*Открыв вкладку "Сеть" и затем запускать сбор данных о сети*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="1fcfc-178">Добавьте URL-адрес в адресной строке браузера с **/api/contact** и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="1fcfc-179">В окне записи сети отображаются сведения передачи.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="1fcfc-180">Обратите внимание, что в ответе тип MIME **application/json**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="1fcfc-181">Это показывает, как формат выходных данных по умолчанию — JSON.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="1fcfc-182">![Просмотр выходных данных запроса веб-API в представлении сети](build-restful-apis-with-aspnet-web-api/_static/image7.png "просмотр выходных данных запроса веб-API в представлении сети")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="1fcfc-183">*Просмотр выходных данных запроса веб-API в представлении сети*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fcfc-184">Поведение по умолчанию Internet Explorer 10 на этом этапе следует задавать, если пользователь хочет сохранить или открыть поток, полученный из вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="1fcfc-185">Выходные данные будут текстовый файл, содержащий результат JSON вызова URL-адрес API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="1fcfc-186">Не отменять диалогового окна, чтобы иметь возможность просматривать содержимое ответа через окно инструментов для разработчиков.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="1fcfc-187">Нажмите кнопку **перейдите к представлению подробные** кнопки для просмотра дополнительных сведений об ответе этот вызов API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="1fcfc-188">![Переключиться в подробное представление](build-restful-apis-with-aspnet-web-api/_static/image8.png "переключитесь в представление сведений")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="1fcfc-189">*Переключиться в подробное представление*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="1fcfc-190">Нажмите кнопку **текст ответа** вкладку, чтобы просмотреть фактический текст ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="1fcfc-191">![Просмотр JSON вывода текста в сетевом мониторе](build-restful-apis-with-aspnet-web-api/_static/image9.png "просмотра JSON вывода текста в сетевом мониторе")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="1fcfc-192">*Просмотр выходного текста JSON в сетевом мониторе*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="1fcfc-193">Задача 3 - Создание связи моделей и дополнять контактные контроллера</span><span class="sxs-lookup"><span data-stu-id="1fcfc-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="1fcfc-194">В этой задаче вы создадите классы контроллера, в которых будет находиться методов API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="1fcfc-195">Щелкните правой кнопкой мыши **моделей** папку и выберите **Add | Класс...**  в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="1fcfc-196">![Добавление новой модели в веб-приложение](build-restful-apis-with-aspnet-web-api/_static/image10.png "Добавление новой модели в веб-приложение")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="1fcfc-197">*Добавление новой модели в веб-приложение*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="1fcfc-198">В **Добавление нового элемента** диалоговое окно, назовите новый файл **Contact.cs** и нажмите кнопку **Add.**</span><span class="sxs-lookup"><span data-stu-id="1fcfc-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="1fcfc-199">![Создание нового файла класса контакта](build-restful-apis-with-aspnet-web-api/_static/image11.png "Создание новый файл класса контакта")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="1fcfc-200">*Создание нового файла класса контакта*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="1fcfc-201">Добавьте следующий выделенный код, который **контакт** класса.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="1fcfc-202">(Code Snippet - *веб-API лаборатории - Ex01 - класса контакта*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="1fcfc-203">В **ContactController** выберите слово **строка** в определение метода **получить** метод и введите слово *контакт*.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="1fcfc-204">После слово вводится в, индикатора будет отображаться в начале слова **контакт**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="1fcfc-205">Либо удерживайте **Ctrl** ключа и нажмите клавишу точки (.) или щелкните значок с помощью мыши, чтобы открыть диалоговое окно помощника в редакторе кода, чтобы автоматически заполнять **с помощью** директив для моделей пространство имен.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![С помощью Intellisense помощь для деклараций пространств имен](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="1fcfc-207">*С помощью Intellisense помощь для деклараций пространств имен*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="1fcfc-208">Изменение кода для **получить** метод, возвращающий массив экземпляров модели контакта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="1fcfc-209">(Code Snippet - *лаборатории API веб - Ex01 - Возврат списка контактов*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="1fcfc-210">Нажмите клавишу **F5** для отладки веб-приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="1fcfc-211">Чтобы просмотреть изменения, внесенные в выходные данные ответа API, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="1fcfc-212">После того, как браузер откроется, нажмите **F12** средств разработчика еще не открытым.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="1fcfc-213">Нажмите кнопку **сети** вкладки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="1fcfc-214">Нажмите клавишу **начать захват** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="1fcfc-215">Добавьте суффикс URL-адрес **/api/contact** на URL-адрес в адресной строке и нажмите клавишу **ввод** ключ.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="1fcfc-216">Нажмите клавишу **перейдите к представлению подробные** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="1fcfc-217">Выберите **текст ответа** вкладки. Вы должны увидеть строку JSON, представляющая сериализованную форму массива экземпляров контакта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="1fcfc-218">![JSON-сериализованных выходных данных вызова метода веб-API, сложных](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON сериализованных выходных данных сложные вызова метода веб-API")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="1fcfc-219">*Выходные данные сериализации JSON сложных вызова метода веб-API*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="1fcfc-220">Задача 4 - извлечения функции в слой служб</span><span class="sxs-lookup"><span data-stu-id="1fcfc-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="1fcfc-221">Эта задача будет показано, как для извлечения функции в слой служб, что упрощает для разработчиков для разделения их функциональность службы от уровня контроллера, тем самым позволяя возможность многократного использования служб, которые фактически выполняющие работу.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="1fcfc-222">Создайте новую папку в корневом каталоге решения и назовите его **служб**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="1fcfc-223">Чтобы сделать это, щелкните правой кнопкой мыши **ContactManager** проекта, выберите **добавить** | **новую папку**, назовите его *служб*.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="1fcfc-224">![Папка служб создание](build-restful-apis-with-aspnet-web-api/_static/image14.png "папку создание служб")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="1fcfc-225">*Создание папки служб*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="1fcfc-226">Щелкните правой кнопкой мыши **служб** папку и выберите **Add | Класс...**  в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="1fcfc-227">![Добавление нового класса в папку службы](build-restful-apis-with-aspnet-web-api/_static/image15.png "Добавление нового класса в папку «службы»")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="1fcfc-228">*Добавление нового класса в папку «службы»*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="1fcfc-229">Когда **Добавление нового элемента** откроется диалоговое окно, назовите новый класс **ContactRepository** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="1fcfc-230">![Создание класса файл, содержащий код для уровня службы репозитория обратитесь к](build-restful-apis-with-aspnet-web-api/_static/image16.png "создания файла класса должен содержать код для уровня службы репозитория контактов")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="1fcfc-231">*Создание класса файл, содержащий код для уровня службы репозитория контактов*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="1fcfc-232">Добавить с помощью директивы **ContactRepository.cs** файл, чтобы включить пространство имен модели.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="1fcfc-233">Добавьте следующий выделенный код, который **ContactRepository.cs** файл для реализации метода GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="1fcfc-234">(Code Snippet - *веб-API лаборатории - Ex01 - репозиторий контактов*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="1fcfc-235">Откройте **ContactController.cs** файла, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="1fcfc-236">Добавьте следующий код с помощью инструкции в раздел объявлений пространства имен файла.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="1fcfc-237">Добавьте следующий выделенный код, который **ContactController.cs** класса добавьте закрытое поле для представления экземпляра репозитория, таким образом, чтобы использовать rest членам класса реализации службы.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="1fcfc-238">(Code Snippet - *лаборатории API - Ex01 - контактные контроллер веб-*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="1fcfc-239">Изменение **получить** метод, поэтому она позволяет использовать службы репозитория контактов.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="1fcfc-240">(Code Snippet - *лаборатории API веб - Ex01 - Возврат списка контактов с помощью репозитория*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="1fcfc-241">Установите точку останова на **ContactController** **получить** определение метода.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="1fcfc-242">![Добавление точек останова к контроллеру контактные](build-restful-apis-with-aspnet-web-api/_static/image17.png "Добавление точек останова к контроллеру контакта")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="1fcfc-243">*Добавление точек останова к контроллеру контакта*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="1fcfc-244">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="1fcfc-245">Когда откроется браузер, нажмите клавишу **F12** открытие средств разработчика.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="1fcfc-246">Нажмите кнопку **сети** вкладки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="1fcfc-247">Нажмите кнопку **начать захват** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="1fcfc-248">Добавьте URL-адрес в адресной строке с суффиксом **/api/contact** и нажмите клавишу **ввод** загрузить контроллер API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="1fcfc-249">Visual Studio 2012 следует останов **получить** начинается выполнение метода.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="1fcfc-250">![Критические в метод Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "критические в метод Get")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="1fcfc-251">*Критические в метод Get*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="1fcfc-252">Чтобы продолжить выполнение, нажмите клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="1fcfc-253">Вернуться назад в Internet Explorer, если он еще не находится в фокусе.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="1fcfc-254">Обратите внимание, окно записи сетевых данных.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-254">Note the network capture window.</span></span>

    <span data-ttu-id="1fcfc-255">![В обозревателе Internet Explorer, отображая результаты вызова веб-API сети](build-restful-apis-with-aspnet-web-api/_static/image19.png "сети в обозревателе Internet Explorer, отображая результаты вызова веб-API")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="1fcfc-256">*Представление сети в Internet Explorer, отображая результаты вызова веб-API*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="1fcfc-257">Нажмите кнопку **перейдите к представлению подробные** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="1fcfc-258">Нажмите кнопку **текст ответа** вкладки. Обратите внимание, выходные данные JSON в вызове API, и как он представляет два контакта, полученный на уровне службы.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="1fcfc-259">![Просмотр выходных данных JSON из веб-API в окне средств разработчика](build-restful-apis-with-aspnet-web-api/_static/image20.png "просмотр выходных данных JSON из веб-API в окне средств разработчика")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="1fcfc-260">*Просмотр выходных данных JSON из веб-API в окне средств разработчика*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="1fcfc-261">Упражнение 2: Создание, чтение и запись веб-API</span><span class="sxs-lookup"><span data-stu-id="1fcfc-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="1fcfc-262">В этом упражнении вы реализуете POST и PUT методы для диспетчера контактов включить его с помощью функции редактирования данных.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="1fcfc-263">Задача 1 - Открытие проекта веб-API</span><span class="sxs-lookup"><span data-stu-id="1fcfc-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="1fcfc-264">В этой задаче будет подготовлена для улучшения проект веб-API, созданный в упражнении 1, таким образом, он может принимать ввод данных пользователем.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="1fcfc-265">Запустите **Visual Studio 2012 Express для Web**, для этого перейдите к **запустить** и тип **VS Express для Web** нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="1fcfc-266">Откройте **начать** решений, расположенный **источника/Ex02-ReadWriteWebAPI/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="1fcfc-267">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="1fcfc-268">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="1fcfc-269">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="1fcfc-270">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="1fcfc-271">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="1fcfc-272">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="1fcfc-273">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="1fcfc-274">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="1fcfc-275">Откройте **Services/ContactRepository.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="1fcfc-276">Задача 2 - Добавление функций сохраняемости данных, к реализации репозиторий контактов</span><span class="sxs-lookup"><span data-stu-id="1fcfc-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="1fcfc-277">В этой задаче будет расширить класс ContactRepository проекта веб-API, созданного в упражнении 1, чтобы его можно сохранить и принимать ввод данных пользователем и новые экземпляры обратитесь в службу.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="1fcfc-278">Добавьте следующую константу для **ContactRepository** класс, представляющий имя имя веб-сервера кэш элемент ключа позже в этом упражнении.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="1fcfc-279">Добавьте конструктор для **ContactRepository** содержит следующий код.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="1fcfc-280">(Code Snippet - *веб-API лаборатории - Ex02 - репозиторий контактов конструктор*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="1fcfc-281">Изменение кода для **GetAllContacts** метод, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="1fcfc-282">(Code Snippet - *лаборатории API веб - Ex02 - получить все контакты*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="1fcfc-283">В этом примере осуществляется для демонстрационных целей и будет использовать кэш веб сервера как на носителе, чтобы значения будут доступны для нескольких клиентов одновременно, а не использовать механизм хранения сеанса или времени жизни запроса хранилища.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="1fcfc-284">Можно было использовать Entity Framework, XML-хранилища или любые другие различные вместо кэше веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="1fcfc-285">Реализуйте новый метод с именем **SaveContact** для **ContactRepository** класс для работа по сохранению контакта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="1fcfc-286">**SaveContact** метод должен принимать один **контакт** и возвращают логическое значение, указывающее успех или неудачу.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="1fcfc-287">(Code Snippet - *веб-API лаборатории - Ex02 - реализация метода SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="1fcfc-288">Упражнение 3: Использование веб-API из HTML-клиента</span><span class="sxs-lookup"><span data-stu-id="1fcfc-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="1fcfc-289">В этом упражнении вы создадите HTML-клиента для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="1fcfc-290">Этот клиент будет упростить обмен данными с веб-API с помощью JavaScript и отображает результаты в браузере с помощью разметки HTML.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="1fcfc-291">Задача 1 - Изменение представления индекса для предоставляют графический интерфейс пользователя для отображения контактов</span><span class="sxs-lookup"><span data-stu-id="1fcfc-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="1fcfc-292">В этой задаче будет изменено представление индекса по умолчанию веб-приложения для поддержки требования отображения списка существующих контактов в HTML-браузером.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="1fcfc-293">Откройте **Visual Studio 2012 Express для Web** если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="1fcfc-294">Откройте **начать** решений, расположенный **источника/Ex03-ConsumingWebAPI/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="1fcfc-295">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="1fcfc-296">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="1fcfc-297">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="1fcfc-298">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="1fcfc-299">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="1fcfc-300">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="1fcfc-301">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="1fcfc-302">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="1fcfc-303">Откройте **Index.cshtml** файл, расположенный в **Views/Home** папки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="1fcfc-304">Замените код HTML в элемент div с идентификатором **текст** , чтобы он выглядел как следующий код.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="1fcfc-305">Добавьте следующий код Javascript в нижней части файла для выполнения запроса HTTP для веб-API.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="1fcfc-306">Откройте **ContactController.cs** файла, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="1fcfc-307">Поместите точку останова на **получить** метод **ContactController** класса.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="1fcfc-308">![Установив точку останова в методе Get контроллера API](build-restful-apis-with-aspnet-web-api/_static/image21.png "установив точку останова в методе Get контроллера API")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="1fcfc-309">*Установив точку останова в методе Get контроллера API*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="1fcfc-310">Нажмите клавишу **F5** для запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-310">Press **F5** to run the project.</span></span> <span data-ttu-id="1fcfc-311">Обозреватель загрузит HTML-документа.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fcfc-312">Убедитесь, что вы подключаетесь к корневой URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="1fcfc-313">После загрузки страницы и выполняет код JavaScript, точка останова будет достигнута и выполнение кода будет приостановлено в контроллере.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="1fcfc-314">![Отладка в вызовы веб-API, с помощью VS Express для Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "отладки в вызовы веб-API, с помощью VS Express для Web")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="1fcfc-315">*Отладка в вызов веб-API, с помощью Visual Studio 2012 Express для Web*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="1fcfc-316">Удалите точку останова и нажмите клавишу **F5** или панель инструментов отладки **Продолжить** кнопку, чтобы продолжить загрузку представление в браузере.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="1fcfc-317">После завершения вызова веб-API должно отобразиться контактов, возвращенных веб-API вызывать отобразить в виде элементов списка в браузере.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="1fcfc-318">![Результаты вызова API, отображаются в браузере в виде элементов списка](build-restful-apis-with-aspnet-web-api/_static/image23.png "результаты вызова API, отображаются в браузере в виде элементов списка")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="1fcfc-319">*Результаты вызова API, отображаются в браузере в виде элементов списка*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="1fcfc-320">Остановите отладку.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="1fcfc-321">Задача 2 - изменение представления индекса обеспечивает графический интерфейс пользователя для создания контактов</span><span class="sxs-lookup"><span data-stu-id="1fcfc-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="1fcfc-322">В этой задаче будет продолжать Изменение представления индекса приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="1fcfc-323">Формы будут добавляться на HTML-страницу, которая будет ввод данных и отправлять их в веб-API для создания нового контакта, и будет создан новый метод контроллера веб-API для сбора даты из графического интерфейса пользователя.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="1fcfc-324">Откройте **ContactController.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="1fcfc-325">Добавьте новый метод в класс контроллера с именем **Post** как показано в следующем коде.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="1fcfc-326">(Code Snippet - *веб-метода Post лаборатории API - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="1fcfc-327">Откройте **Index.cshtml** файла в Visual Studio, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="1fcfc-328">Добавьте приведенный ниже код HTML в файл сразу после неупорядоченного списка, добавленного в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="1fcfc-329">В элементе скрипта, в нижней части документа добавьте выделенный ниже код для обработки события нажатия кнопки, которые будет отправлять данные веб-API с помощью вызова HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="1fcfc-330">В **ContactController.cs**, поместите точку останова на **Post** метод.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="1fcfc-331">Нажмите клавишу **F5** для запуска приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="1fcfc-332">После загрузки страницы в браузере, введите имя нового контакта и идентификатор и нажмите кнопку **Сохранить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="1fcfc-333">![Загружен документ клиента HTML в браузере](build-restful-apis-with-aspnet-web-api/_static/image24.png "загружен документ клиента HTML в браузере")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="1fcfc-334">*Клиент HTML-документа, загруженной в браузере*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="1fcfc-335">Когда в окне отладчика прерывается **Post** метод, просмотр свойств **обратитесь в службу** параметра.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="1fcfc-336">Значения должны соответствовать данные, введенные в форме.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="1fcfc-337">![Объект контакт, отправляются в веб-API из клиента](build-restful-apis-with-aspnet-web-api/_static/image25.png "объектов Contact, отправляемых веб-API из клиента")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="1fcfc-338">*Объект контакт, отправляются в веб-API из клиента*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="1fcfc-339">Шаг при помощи метода отладчик до **ответа** переменная создана.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="1fcfc-340">После проверки в **"Локальные"** окно в отладчике, вы увидите что заданы все свойства.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="1fcfc-341">![После создания в отладчике ответ](build-restful-apis-with-aspnet-web-api/_static/image26.png "ответа, после создания в отладчике")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="1fcfc-342">*Ответ после создания в отладчике*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="1fcfc-343">Если нажать клавишу **F5** или нажмите кнопку **Продолжить** в отладчике запрос будет выполнен.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="1fcfc-344">После переключения обратно в браузер, новый контакт был добавлен в список контактов, хранящихся в **ContactRepository** реализации.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="1fcfc-345">![Браузер отражает успешного создания нового экземпляра контактные](build-restful-apis-with-aspnet-web-api/_static/image27.png "браузер отражает успешного создания нового экземпляра контакта")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="1fcfc-346">*Браузер отражает успешного создания нового экземпляра контакта*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="1fcfc-347">Кроме того, можно развернуть это приложение для Azure следующих [приложение C: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1fcfc-348">Сводка</span><span class="sxs-lookup"><span data-stu-id="1fcfc-348">Summary</span></span>

<span data-ttu-id="1fcfc-349">Это лабораторное занятие был представлен на новую платформу веб-API ASP.NET и реализации Web API-интерфейсов RESTful с помощью платформы.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="1fcfc-350">На этой странице можно создать новый репозиторий, который обеспечивает сохраняемость данных, используя любое количество механизмы и привязывания этой службы вместо того, простой, приведено в качестве примера в этой лабораторной работе.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="1fcfc-351">Веб-API поддерживает несколько дополнительных функций, таких как включение подключения от клиентов отличающийся от HTML, написанных на любом языке, поддерживающем HTTP и JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="1fcfc-352">Возможность размещения веб-API за пределами типичный веб-приложения можно также, а также является возможность создавать собственные форматы сериализации.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="1fcfc-353">Веб-узла ASP.NET имеет выделенный в веб-API ASP.NET Framework в область [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="1fcfc-354">Этот сайт будет продолжать последние сведения, примеры и новостей, относящихся к веб-API, поэтому она регулярно проверяйте Если вы хотите более подробно рассмотрим искусством создания пользовательского веб-API, доступных для framework практически с любого устройства или разработки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="1fcfc-355">Приложение а. Использование фрагментов кода</span><span class="sxs-lookup"><span data-stu-id="1fcfc-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="1fcfc-356">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="1fcfc-357">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="1fcfc-358">![Фрагменты кода Visual Studio, чтобы вставить код в проект](build-restful-apis-with-aspnet-web-api/_static/image28.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="1fcfc-359">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="1fcfc-360">Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="1fcfc-361">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="1fcfc-362">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="1fcfc-363">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="1fcfc-364">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="1fcfc-365">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="1fcfc-366">![Начните вводить имя фрагмента](build-restful-apis-with-aspnet-web-api/_static/image29.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="1fcfc-367">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="1fcfc-368">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](build-restful-apis-with-aspnet-web-api/_static/image30.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="1fcfc-369">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="1fcfc-370">![Снова нажмите клавишу Tab и фрагмент будет расширяться](build-restful-apis-with-aspnet-web-api/_static/image31.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="1fcfc-371">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="1fcfc-372">Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)</span><span class="sxs-lookup"><span data-stu-id="1fcfc-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="1fcfc-373">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="1fcfc-374">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="1fcfc-375">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="1fcfc-376">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](build-restful-apis-with-aspnet-web-api/_static/image32.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="1fcfc-377">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="1fcfc-378">![Выберите соответствующий фрагмент из списка, щелкнув по ней](build-restful-apis-with-aspnet-web-api/_static/image33.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="1fcfc-379">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="1fcfc-380">Приложение б. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="1fcfc-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="1fcfc-381">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="1fcfc-382">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="1fcfc-383">Перейдите к [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="1fcfc-384">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="1fcfc-385">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-385">Click on **Install Now**.</span></span> <span data-ttu-id="1fcfc-386">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="1fcfc-387">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="1fcfc-388">![Установка Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="1fcfc-389">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="1fcfc-390">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="1fcfc-392">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="1fcfc-393">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-393">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="1fcfc-395">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-395">*Installation progress*</span></span>
6. <span data-ttu-id="1fcfc-396">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-396">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="1fcfc-398">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-398">*Installation completed*</span></span>
7. <span data-ttu-id="1fcfc-399">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="1fcfc-400">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="1fcfc-402">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="1fcfc-403">Приложение c. публикации приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="1fcfc-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="1fcfc-404">В этом приложении показано, как создать новый веб-сайт на портале Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Azure.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="1fcfc-405">Задача 1 - Создание нового веб-сайта с помощью портала Azure</span><span class="sxs-lookup"><span data-stu-id="1fcfc-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="1fcfc-406">Перейдите к [портала управления Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fcfc-407">С помощью Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="1fcfc-408">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="1fcfc-409">![Войдите на портал Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Войдите на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="1fcfc-410">*Войдите на портал*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="1fcfc-411">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="1fcfc-412">![Создание нового веб-сайта](build-restful-apis-with-aspnet-web-api/_static/image40.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="1fcfc-413">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="1fcfc-414">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="1fcfc-415">Затем выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="1fcfc-416">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fcfc-417">Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="1fcfc-418">Быстрое создание позволяет развернуть завершенное веб-приложения в Azure из вне портала.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="1fcfc-419">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="1fcfc-420">![Создание нового веб-сайта, с помощью быстрого создания](build-restful-apis-with-aspnet-web-api/_static/image41.png "создания нового веб-сайта, с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="1fcfc-421">*Создание нового веб-сайта, с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="1fcfc-422">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="1fcfc-423">После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="1fcfc-424">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="1fcfc-425">![Выбрав новый веб-сайт](build-restful-apis-with-aspnet-web-api/_static/image42.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="1fcfc-426">*Выбрав новый веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="1fcfc-427">![Веб-сайт работал](build-restful-apis-with-aspnet-web-api/_static/image43.png "веб-узлом")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="1fcfc-428">*Веб-сайт под управлением*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-428">*Web site running*</span></span>
6. <span data-ttu-id="1fcfc-429">Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="1fcfc-430">![Открытие страницы управления веб-сайт](build-restful-apis-with-aspnet-web-api/_static/image44.png "Открытие страницы управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="1fcfc-431">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="1fcfc-432">В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fcfc-433">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения в Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="1fcfc-434">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="1fcfc-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений в Azure.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="1fcfc-436">![Профиль публикации веб-сайт загрузки](build-restful-apis-with-aspnet-web-api/_static/image45.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="1fcfc-437">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="1fcfc-438">Скачайте файл профиля публикации в известном месте.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="1fcfc-439">Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения в Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="1fcfc-440">![Сохранение файла профиля публикации](build-restful-apis-with-aspnet-web-api/_static/image46.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="1fcfc-441">*Сохранение файла профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="1fcfc-442">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="1fcfc-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="1fcfc-443">Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="1fcfc-444">Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="1fcfc-445">Вам потребуется сервер базы данных SQL для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="1fcfc-446">Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Azure на **баз данных Sql** | **серверы** | **мониторинга сервера**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="1fcfc-447">Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="1fcfc-448">Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="1fcfc-449">Не создавайте базы данных, так как он будет создан в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="1fcfc-450">![Панель мониторинга базы данных SQL Server](build-restful-apis-with-aspnet-web-api/_static/image47.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="1fcfc-451">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="1fcfc-452">В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="1fcfc-453">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Добавление IP-адрес клиента](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="1fcfc-455">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="1fcfc-456">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="1fcfc-458">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="1fcfc-459">Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="1fcfc-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="1fcfc-460">Вернитесь к решению для ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="1fcfc-461">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="1fcfc-462">![Публикация приложения](build-restful-apis-with-aspnet-web-api/_static/image51.png "публикации приложения")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="1fcfc-463">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="1fcfc-464">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="1fcfc-465">![Импорт профиля публикации](build-restful-apis-with-aspnet-web-api/_static/image52.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="1fcfc-466">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="1fcfc-467">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-467">Click **Validate Connection**.</span></span> <span data-ttu-id="1fcfc-468">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fcfc-469">Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="1fcfc-470">![Проверка подключения](build-restful-apis-with-aspnet-web-api/_static/image53.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="1fcfc-471">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-471">*Validating connection*</span></span>
4. <span data-ttu-id="1fcfc-472">В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="1fcfc-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="1fcfc-473">![Конфигурация веб-развертывания](build-restful-apis-with-aspnet-web-api/_static/image54.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="1fcfc-474">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="1fcfc-475">Настройте подключение к базе данных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1fcfc-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="1fcfc-476">В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="1fcfc-477">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="1fcfc-478">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="1fcfc-479">Введите новое имя базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="1fcfc-480">![Настройка строки подключения назначения](build-restful-apis-with-aspnet-web-api/_static/image55.png "Настройка строка соединения с назначением")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="1fcfc-481">*Настройка строки подключения назначения*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="1fcfc-482">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-482">Then click **OK**.</span></span> <span data-ttu-id="1fcfc-483">При появлении запроса на создание базы данных нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="1fcfc-484">![Создание базы данных](build-restful-apis-with-aspnet-web-api/_static/image56.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="1fcfc-485">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-485">*Creating the database*</span></span>
7. <span data-ttu-id="1fcfc-486">В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="1fcfc-487">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-487">Then click **Next**.</span></span>

    <span data-ttu-id="1fcfc-488">![Строка подключения, указывающий на базу данных SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "строку подключения, указывающий на базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="1fcfc-489">*Строка подключения, указывающий на базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="1fcfc-490">В **предварительной версии** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="1fcfc-491">![Публикация веб-приложения](build-restful-apis-with-aspnet-web-api/_static/image58.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="1fcfc-492">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="1fcfc-493">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="1fcfc-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="1fcfc-494">![Приложение опубликовано в Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="1fcfc-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="1fcfc-495">*Приложение будет публиковаться в Azure*</span><span class="sxs-lookup"><span data-stu-id="1fcfc-495">*Application published to Azure*</span></span>
