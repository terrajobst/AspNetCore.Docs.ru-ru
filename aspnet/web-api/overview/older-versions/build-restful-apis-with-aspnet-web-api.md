---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Построение RESTful API-интерфейсы с веб-API ASP.NET | Документы Microsoft
author: rick-anderson
description: В последние годы он становится ясно, что HTTP является не просто для подготовки HTML-страницы. Это также мощную платформу для построения веб-API, с помощью o несколько...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 320409cd395384a608a07307a56d18105d45de14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="b18da-104">Построение RESTful API-интерфейсы с веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b18da-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="b18da-105">По [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b18da-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="b18da-106">В последние годы он становится ясно, что HTTP является не просто для подготовки HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="b18da-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="b18da-107">Это также мощную платформу для построения веб-API, с помощью несколько команд (GET, POST и т. д.) и простой понятия, например *идентификаторы URI* и *заголовки*.</span><span class="sxs-lookup"><span data-stu-id="b18da-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="b18da-108">Веб-API ASP.NET — это набор компонентов, которые упрощают программирование HTTP.</span><span class="sxs-lookup"><span data-stu-id="b18da-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="b18da-109">Так как она построена на основе среды выполнения ASP.NET MVC, веб-API автоматически обрабатывает сведения низкого уровня транспорта протокола HTTP.</span><span class="sxs-lookup"><span data-stu-id="b18da-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="b18da-110">В то же время веб-API естественным образом предоставляет модель программирования HTTP.</span><span class="sxs-lookup"><span data-stu-id="b18da-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="b18da-111">На самом деле является одна из целей веб-API *не* абстрагирующих реальность HTTP.</span><span class="sxs-lookup"><span data-stu-id="b18da-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="b18da-112">В результате веб-API, гибкие и легко расширить.</span><span class="sxs-lookup"><span data-stu-id="b18da-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="b18da-113">В этой практической будет использовать веб-API для создания простой API REST для приложения диспетчера контактов.</span><span class="sxs-lookup"><span data-stu-id="b18da-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="b18da-114">Клиент для использования API, производится построение.</span><span class="sxs-lookup"><span data-stu-id="b18da-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="b18da-115">Архитектурный стиль REST показала эффективный способ использовать HTTP -, несмотря на то, что не допускаются только подход к HTTP.</span><span class="sxs-lookup"><span data-stu-id="b18da-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="b18da-116">Диспетчер контактов будет предоставлять категории RESTful для получения списка, добавление и удаление контактов, в частности.</span><span class="sxs-lookup"><span data-stu-id="b18da-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="b18da-117">Эта лаборатория требует основные HTTP, REST и предполагается наличие базовых знаний рабочий HTML, JavaScript и jQuery.</span><span class="sxs-lookup"><span data-stu-id="b18da-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="b18da-118">Веб-сайт ASP.NET имеет область для .NET framework веб-API ASP.NET с [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="b18da-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="b18da-119">Этот сайт продолжить предоставлять последние сведения, примеры и новости, относящиеся к веб-API, поэтому он регулярно проверять Если вы хотите более подробно рассмотрим иллюстрации создания пользовательского веб-API для framework практически с любого устройства или для разработки.</span><span class="sxs-lookup"><span data-stu-id="b18da-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="b18da-120">Веб-API, аналогично ASP.NET MVC 4 ASP.NET имеет большую гибкость с точки зрения разделения уровень службы на контроллерах, позволяя использовать несколько из доступных платформ внедрения зависимостей довольно просто.</span><span class="sxs-lookup"><span data-stu-id="b18da-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="b18da-121">Отсутствует в библиотеке MSDN, в котором показано, как использовать Ninject для внедрения зависимости в проекте веб-API ASP.NET, его можно загрузить из хороший пример [здесь](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="b18da-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="b18da-122">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="b18da-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b18da-123">Цели</span><span class="sxs-lookup"><span data-stu-id="b18da-123">Objectives</span></span>

<span data-ttu-id="b18da-124">В этой практической вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="b18da-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="b18da-125">Реализовать RESTful веб-API</span><span class="sxs-lookup"><span data-stu-id="b18da-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="b18da-126">Вызова API из HTML-клиента</span><span class="sxs-lookup"><span data-stu-id="b18da-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b18da-127">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="b18da-127">Prerequisites</span></span>

<span data-ttu-id="b18da-128">Для завершения этой практической требуется следующее:</span><span class="sxs-lookup"><span data-stu-id="b18da-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="b18da-129">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложении Б](#AppendixB) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="b18da-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b18da-130">Установка</span><span class="sxs-lookup"><span data-stu-id="b18da-130">Setup</span></span>

<span data-ttu-id="b18da-131">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="b18da-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="b18da-132">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b18da-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b18da-133">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="b18da-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b18da-134">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b18da-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b18da-135">Упражнения</span><span class="sxs-lookup"><span data-stu-id="b18da-135">Exercises</span></span>

<span data-ttu-id="b18da-136">Данная практическая работа включает следующее упражнение:</span><span class="sxs-lookup"><span data-stu-id="b18da-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="b18da-137">: Упражнение 1 только для чтения веб-API</span><span class="sxs-lookup"><span data-stu-id="b18da-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="b18da-138">Упражнение 2: Создание чтения и записи веб-API</span><span class="sxs-lookup"><span data-stu-id="b18da-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="b18da-139">Упражнение 3: Использование веб-API из HTML-клиента</span><span class="sxs-lookup"><span data-stu-id="b18da-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="b18da-140">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="b18da-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b18da-141">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="b18da-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b18da-142">Предполагаемое время для выполнения этого занятия: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="b18da-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="b18da-143">: Упражнение 1 только для чтения веб-API</span><span class="sxs-lookup"><span data-stu-id="b18da-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="b18da-144">В этом упражнении вы реализовать методы GET только для чтения для диспетчера контактов.</span><span class="sxs-lookup"><span data-stu-id="b18da-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="b18da-145">Задача 1. Создание проекта API</span><span class="sxs-lookup"><span data-stu-id="b18da-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="b18da-146">В этой задаче будет использовать новые шаблоны веб-проектов ASP.NET для создания веб-приложения веб-API.</span><span class="sxs-lookup"><span data-stu-id="b18da-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="b18da-147">Запустите **Visual Studio 2012 Express для Web**, для этого перейдите к **запустить** и тип **VS Express для Web** нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="b18da-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="b18da-148">Из **файл** последовательно выберите пункты **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="b18da-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="b18da-149">Выберите **Visual C# | Web** тип из представления дерева проекта типа проекта, а затем выберите **веб-приложение ASP.NET MVC 4** тип проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="b18da-150">Установите для свойства проекта **имя** для *ContactManager* и **имя решения** для *начать*, нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b18da-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="b18da-151">![Создание нового ASP.NET MVC 4.0 проект веб-приложения](build-restful-apis-with-aspnet-web-api/_static/image1.png "создание новых ASP.NET MVC 4.0 проект веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="b18da-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="b18da-152">*Создание нового ASP.NET MVC 4.0 проект веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="b18da-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="b18da-153">В диалоговом окне тип проекта ASP.NET MVC 4, выберите **веб-API** тип проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="b18da-154">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b18da-154">Click **OK**.</span></span>

    <span data-ttu-id="b18da-155">![Указывает тип проекта веб-API](build-restful-apis-with-aspnet-web-api/_static/image2.png "указывает тип проекта веб-API")</span><span class="sxs-lookup"><span data-stu-id="b18da-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="b18da-156">*Указывает тип проекта веб-API*</span><span class="sxs-lookup"><span data-stu-id="b18da-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="b18da-157">Задача 2 - Создание контроллеров API диспетчера контактов</span><span class="sxs-lookup"><span data-stu-id="b18da-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="b18da-158">В этой задаче вы создадите классы контроллера, в которых будут храниться методов API.</span><span class="sxs-lookup"><span data-stu-id="b18da-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="b18da-159">Удалите файл с именем **ValuesController.cs** в **контроллеров** папку из проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="b18da-160">Щелкните правой кнопкой мыши **контроллеров** папки в проект и выберите **добавить | Контроллер** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b18da-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="b18da-161">![Добавление в проект новый контроллер](build-restful-apis-with-aspnet-web-api/_static/image3.png "новый контроллер добавляется в проект")</span><span class="sxs-lookup"><span data-stu-id="b18da-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="b18da-162">*Добавление в проект новый контроллер*</span><span class="sxs-lookup"><span data-stu-id="b18da-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="b18da-163">В **добавления контроллера** диалоговое окно, которое отображается, выберите **пустой контроллер API** меню шаблон.</span><span class="sxs-lookup"><span data-stu-id="b18da-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="b18da-164">Имя класса контроллера **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="b18da-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="b18da-165">Нажмите кнопку **Add.**</span><span class="sxs-lookup"><span data-stu-id="b18da-165">Then, click **Add.**</span></span>

    <span data-ttu-id="b18da-166">![С помощью диалогового окна добавления контроллера, чтобы создать новый контроллер веб-API](build-restful-apis-with-aspnet-web-api/_static/image4.png "с помощью диалогового окна добавления контроллера, чтобы создать новый контроллер веб-API")</span><span class="sxs-lookup"><span data-stu-id="b18da-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="b18da-167">*С помощью диалогового окна добавления контроллера, чтобы создать новый контроллер веб-API*</span><span class="sxs-lookup"><span data-stu-id="b18da-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="b18da-168">Добавьте следующий код в **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="b18da-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="b18da-169">(Фрагмент - кода *метод API Get лаборатории API веб - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="b18da-170">Нажмите клавишу **F5** отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="b18da-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="b18da-171">Должны появиться домашнюю страницу по умолчанию для проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="b18da-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="b18da-172">![Домашняя страница по умолчанию, приложения веб-API ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "домашнюю страницу по умолчанию приложения ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="b18da-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="b18da-173">*Домашняя страница по умолчанию, приложения веб-API ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="b18da-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="b18da-174">В окне Internet Explorer нажмите **F12** раздел, чтобы открыть **средств разработчика** окна.</span><span class="sxs-lookup"><span data-stu-id="b18da-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="b18da-175">Нажмите кнопку **сети** , а затем щелкните **начать захват** кнопку, чтобы начать отслеживать сетевой трафик в окно.</span><span class="sxs-lookup"><span data-stu-id="b18da-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="b18da-176">![Открыв вкладку "Сеть" и инициирует сбор по сети](build-restful-apis-with-aspnet-web-api/_static/image6.png "открыв вкладку "Сеть" и инициирует сбор по сети")</span><span class="sxs-lookup"><span data-stu-id="b18da-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="b18da-177">*Открыв вкладку "Сеть" и инициации записи сетевого трафика*</span><span class="sxs-lookup"><span data-stu-id="b18da-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="b18da-178">Добавьте URL-адрес в адресной строке браузера с **/api/контакт** и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="b18da-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="b18da-179">Передачи сведений будут отображаться в окне сбора данных сети.</span><span class="sxs-lookup"><span data-stu-id="b18da-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="b18da-180">Обратите внимание, что тип MIME ответа **приложение/json**.</span><span class="sxs-lookup"><span data-stu-id="b18da-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="b18da-181">Этот пример демонстрирует, как выходного формата по умолчанию является JSON.</span><span class="sxs-lookup"><span data-stu-id="b18da-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="b18da-182">![Просмотр выходных данных запроса веб-API в представлении сети](build-restful-apis-with-aspnet-web-api/_static/image7.png "просмотр выходных данных запроса веб-API в представлении сети")</span><span class="sxs-lookup"><span data-stu-id="b18da-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="b18da-183">*Просмотр выходных данных запроса веб-API в представлении сети*</span><span class="sxs-lookup"><span data-stu-id="b18da-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b18da-184">Поведение по умолчанию Internet Explorer 10 на этом этапе будет запрашивать, если хотите сохранить или открыть поток, полученный из вызова веб-API пользователя.</span><span class="sxs-lookup"><span data-stu-id="b18da-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="b18da-185">Выходные данные будут текстовый файл, содержащий результат JSON вызова API URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="b18da-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="b18da-186">Чтобы иметь возможность просматривать содержимое ответа через окно инструментов разработчики не отменяйте диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="b18da-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="b18da-187">Нажмите кнопку **перейдите в подробном представлении** кнопку, чтобы получить дополнительные сведения об ответе этот вызов API.</span><span class="sxs-lookup"><span data-stu-id="b18da-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="b18da-188">![Переключитесь в представление подробные](build-restful-apis-with-aspnet-web-api/_static/image8.png "переключитесь в Просмотр подробностей")</span><span class="sxs-lookup"><span data-stu-id="b18da-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="b18da-189">*Переключитесь в подробном представлении*</span><span class="sxs-lookup"><span data-stu-id="b18da-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="b18da-190">Нажмите кнопку **текст ответа** вкладку, чтобы просмотреть фактический текст ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="b18da-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="b18da-191">![Просмотр JSON вывода текста в сетевом мониторе](build-restful-apis-with-aspnet-web-api/_static/image9.png "просмотра JSON вывода текста в сетевом мониторе")</span><span class="sxs-lookup"><span data-stu-id="b18da-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="b18da-192">*Просмотр вывода текста JSON в сетевом мониторе*</span><span class="sxs-lookup"><span data-stu-id="b18da-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="b18da-193">Задача 3. Создание связи моделей и расширяют контактов контроллера</span><span class="sxs-lookup"><span data-stu-id="b18da-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="b18da-194">В этой задаче вы создадите классы контроллера, в которых будут храниться методов API.</span><span class="sxs-lookup"><span data-stu-id="b18da-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="b18da-195">Щелкните правой кнопкой мыши **моделей** папку и выберите **добавить | Класс...**  в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b18da-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="b18da-196">![Добавление новой модели веб-приложение](build-restful-apis-with-aspnet-web-api/_static/image10.png "Добавление новой модели веб-приложение")</span><span class="sxs-lookup"><span data-stu-id="b18da-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="b18da-197">*Добавление новой модели веб-приложение*</span><span class="sxs-lookup"><span data-stu-id="b18da-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="b18da-198">В **Добавление нового элемента** диалогового окна, задайте имя для нового файла **Contact.cs** и нажмите кнопку **Add.**</span><span class="sxs-lookup"><span data-stu-id="b18da-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="b18da-199">![Создание нового файла класса контакта](build-restful-apis-with-aspnet-web-api/_static/image11.png "Создание нового файла класса контакта")</span><span class="sxs-lookup"><span data-stu-id="b18da-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="b18da-200">*Создание нового файла класса контакта*</span><span class="sxs-lookup"><span data-stu-id="b18da-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="b18da-201">Добавьте следующий выделенный код в **контакт** класса.</span><span class="sxs-lookup"><span data-stu-id="b18da-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="b18da-202">(Фрагмент - кода *веб-класса контакта лаборатории API - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. <span data-ttu-id="b18da-203">В **ContactController** класса, выделите слово **строка** в определении метода **получить** метод и введите слово *контакт*.</span><span class="sxs-lookup"><span data-stu-id="b18da-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="b18da-204">После ввода слова в, индикатор будет отображаться в начале слова **контакт**.</span><span class="sxs-lookup"><span data-stu-id="b18da-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="b18da-205">Либо удерживайте **Ctrl** и нажмите клавишу точки (.) или щелкните значок, используя мышь, чтобы открыть диалоговое окно помощника в редакторе кода для автоматического заполнения **с помощью** директив для моделей пространство имен.</span><span class="sxs-lookup"><span data-stu-id="b18da-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![С помощью Intellisense помощь для объявления пространства имен](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="b18da-207">*С помощью Intellisense помощь для объявления пространства имен*</span><span class="sxs-lookup"><span data-stu-id="b18da-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="b18da-208">Изменение кода для **получить** метод, возвращающий массив экземпляров обратитесь к модели.</span><span class="sxs-lookup"><span data-stu-id="b18da-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="b18da-209">(Фрагмент - кода *Возврат списка контактов лаборатории API веб - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="b18da-210">Нажмите клавишу **F5** для отладки веб-приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="b18da-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="b18da-211">Чтобы просмотреть изменения, внесенные в выходные данные ответа API-интерфейса, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="b18da-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="b18da-212">Как только браузер откроется, нажмите клавишу **F12** средства разработчика еще не открытым.</span><span class="sxs-lookup"><span data-stu-id="b18da-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="b18da-213">Нажмите кнопку **сети** вкладки.</span><span class="sxs-lookup"><span data-stu-id="b18da-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="b18da-214">Нажмите клавишу **начать захват** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b18da-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="b18da-215">Добавьте суффикс URL-адрес **/api/контакт** URL-адрес в адресной строке и нажмите клавишу **ввод** ключа.</span><span class="sxs-lookup"><span data-stu-id="b18da-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="b18da-216">Нажмите клавишу **перейдите в подробном представлении** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b18da-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="b18da-217">Выберите **текст ответа** вкладки. Должна появиться строка JSON, представляющая сериализованную форму массив экземпляров контакта.</span><span class="sxs-lookup"><span data-stu-id="b18da-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="b18da-218">![Выходные данные сложных вызова метода веб-API сериализации JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "вывода сложных вызова метода веб-API сериализации JSON")</span><span class="sxs-lookup"><span data-stu-id="b18da-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="b18da-219">*Выходные данные сериализован в JSON сложных вызова метода веб-API*</span><span class="sxs-lookup"><span data-stu-id="b18da-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="b18da-220">Задача 4 - извлечение функции разбиваются на уровне службы</span><span class="sxs-lookup"><span data-stu-id="b18da-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="b18da-221">Эта задача будет показано извлечение функции разбиваются на уровне службы, чтобы облегчить разработчикам разделить их функциональные возможности службы от уровня контроллера, что делает возможным повторное использование служб, которые фактически выполняют работу.</span><span class="sxs-lookup"><span data-stu-id="b18da-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="b18da-222">Создайте новую папку в корневой папке решение и назовите его **службы**.</span><span class="sxs-lookup"><span data-stu-id="b18da-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="b18da-223">Для этого щелкните правой кнопкой мыши **ContactManager** проекта, выберите **добавить** | **новую папку**, назовите его *службы*.</span><span class="sxs-lookup"><span data-stu-id="b18da-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="b18da-224">![Создание папок служб](build-restful-apis-with-aspnet-web-api/_static/image14.png "папка создание служб")</span><span class="sxs-lookup"><span data-stu-id="b18da-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="b18da-225">*Создание папки для служб*</span><span class="sxs-lookup"><span data-stu-id="b18da-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="b18da-226">Щелкните правой кнопкой мыши **службы** папку и выберите **добавить | Класс...**  в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="b18da-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="b18da-227">![Добавление нового класса в папку «службы»](build-restful-apis-with-aspnet-web-api/_static/image15.png "Добавление нового класса в папку «службы»")</span><span class="sxs-lookup"><span data-stu-id="b18da-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="b18da-228">*Добавление нового класса в папку «службы»*</span><span class="sxs-lookup"><span data-stu-id="b18da-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="b18da-229">Когда **Добавление нового элемента** откроется диалоговое окно, имя для нового класса **ContactRepository** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="b18da-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="b18da-230">![Создания файла класса, чтобы он содержал код для уровня службы репозитория обратитесь к](build-restful-apis-with-aspnet-web-api/_static/image16.png "создания файла класса, чтобы он содержал код для уровня службы репозитория обратитесь к")</span><span class="sxs-lookup"><span data-stu-id="b18da-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="b18da-231">*Создание класса файл, содержащий код для уровня службы репозитория контактов*</span><span class="sxs-lookup"><span data-stu-id="b18da-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="b18da-232">Добавить с помощью директивы **ContactRepository.cs** файла следует включить пространство имен модели.</span><span class="sxs-lookup"><span data-stu-id="b18da-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. <span data-ttu-id="b18da-233">Добавьте следующий выделенный код в **ContactRepository.cs** файл для реализации метода GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="b18da-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="b18da-234">(Фрагмент - кода *веб-репозитория контактов лаборатории API - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="b18da-235">Откройте **ContactController.cs** файл, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="b18da-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="b18da-236">Добавьте следующий код с помощью инструкции раздел объявления пространства имен в файле.</span><span class="sxs-lookup"><span data-stu-id="b18da-236">Add the following using statement to the namespace declaration section of the file.</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. <span data-ttu-id="b18da-237">Добавьте следующий выделенный код в **ContactController.cs** класса добавьте закрытое поле для представления экземпляра репозитория, чтобы использовать все доступные членам класса реализации службы.</span><span class="sxs-lookup"><span data-stu-id="b18da-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="b18da-238">(Фрагмент - кода *лаборатории API - Ex01 - контактные контроллер веб-*)</span><span class="sxs-lookup"><span data-stu-id="b18da-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="b18da-239">Изменение **получить** метод, поэтому она позволяет использовать службы репозитория контактов.</span><span class="sxs-lookup"><span data-stu-id="b18da-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="b18da-240">(Фрагмент - кода *Возврат списка контактов через репозитории лаборатории API веб - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="b18da-241">Установите точку останова на **ContactController** **получить** определение метода.</span><span class="sxs-lookup"><span data-stu-id="b18da-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="b18da-242">![Добавление точек останова контакта контроллер](build-restful-apis-with-aspnet-web-api/_static/image17.png "добавления точки останова к контроллеру контакта")</span><span class="sxs-lookup"><span data-stu-id="b18da-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="b18da-243">*Добавление точки останова к контроллеру контакта*</span><span class="sxs-lookup"><span data-stu-id="b18da-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="b18da-244">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="b18da-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="b18da-245">Когда откроется браузер, нажмите клавишу **F12** открытие средств разработчика.</span><span class="sxs-lookup"><span data-stu-id="b18da-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="b18da-246">Нажмите кнопку **сети** вкладки.</span><span class="sxs-lookup"><span data-stu-id="b18da-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="b18da-247">Нажмите кнопку **начать захват** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b18da-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="b18da-248">Добавьте URL-адрес в адресной строке с суффиксом **/api/контакт** и нажмите клавишу **ввод** для загрузки контроллер API.</span><span class="sxs-lookup"><span data-stu-id="b18da-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="b18da-249">Visual Studio 2012 следует останов **получить** начинается выполнение метода.</span><span class="sxs-lookup"><span data-stu-id="b18da-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="b18da-250">![Критические внутри метода Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "критические внутри метода Get")</span><span class="sxs-lookup"><span data-stu-id="b18da-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="b18da-251">*Критические внутри метода Get*</span><span class="sxs-lookup"><span data-stu-id="b18da-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="b18da-252">Чтобы продолжить выполнение, нажмите клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="b18da-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="b18da-253">Вернуться назад в Internet Explorer, если он еще не находится в фокусе.</span><span class="sxs-lookup"><span data-stu-id="b18da-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="b18da-254">Обратите внимание, окно записи сетевых данных.</span><span class="sxs-lookup"><span data-stu-id="b18da-254">Note the network capture window.</span></span>

    <span data-ttu-id="b18da-255">![Сетевой представления в обозревателе Internet Explorer, показывающий результаты вызова веб-API](build-restful-apis-with-aspnet-web-api/_static/image19.png "сети представления в обозревателе Internet Explorer, показывающий результаты вызова веб-API")</span><span class="sxs-lookup"><span data-stu-id="b18da-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="b18da-256">*Представление сети в Internet Explorer, показывающий результаты вызова веб-API*</span><span class="sxs-lookup"><span data-stu-id="b18da-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="b18da-257">Нажмите кнопку **перейдите в подробном представлении** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b18da-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="b18da-258">Нажмите кнопку **текст ответа** вкладки. Обратите внимание, выходные данные JSON вызова интерфейса API, и как он представляет два контакта получить уровень служб.</span><span class="sxs-lookup"><span data-stu-id="b18da-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="b18da-259">![Просмотр выходных данных JSON из веб-API в окно средств разработчика](build-restful-apis-with-aspnet-web-api/_static/image20.png "просмотр выходных данных JSON из веб-API в окно средств разработчика")</span><span class="sxs-lookup"><span data-stu-id="b18da-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="b18da-260">*Просмотр выходных данных JSON из веб-API в окно средств разработчика*</span><span class="sxs-lookup"><span data-stu-id="b18da-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="b18da-261">Упражнение 2: Создание чтения и записи веб-API</span><span class="sxs-lookup"><span data-stu-id="b18da-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="b18da-262">В этом упражнении будет реализовывать POST и PUT методы для диспетчера контактов для его включения возможности редактирования данных.</span><span class="sxs-lookup"><span data-stu-id="b18da-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="b18da-263">Задача 1. Открытие проекта веб-API</span><span class="sxs-lookup"><span data-stu-id="b18da-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="b18da-264">В этой задаче будет подготовить для улучшения проект веб-API, которые создаются в упражнении 1, поэтому он может принимать ввод данных пользователем.</span><span class="sxs-lookup"><span data-stu-id="b18da-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="b18da-265">Запустите **Visual Studio 2012 Express для Web**, для этого перейдите к **запустить** и тип **VS Express для Web** нажмите клавишу **ввод**.</span><span class="sxs-lookup"><span data-stu-id="b18da-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="b18da-266">Откройте **начать** решений, расположенный **источника/Ex02-ReadWriteWebAPI/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="b18da-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="b18da-267">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="b18da-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b18da-268">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="b18da-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b18da-269">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b18da-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b18da-270">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="b18da-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b18da-271">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="b18da-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b18da-272">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b18da-273">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b18da-274">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="b18da-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b18da-275">Откройте **Services/ContactRepository.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="b18da-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="b18da-276">Задача 2 - Добавление возможности сохранения данных для реализации репозитория контактов</span><span class="sxs-lookup"><span data-stu-id="b18da-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="b18da-277">В этой задаче будет расширить класс ContactRepository проекта веб-API, созданные в упражнении 1, чтобы его можно сохранять и принимать ввод данных пользователем и новые экземпляры контакта.</span><span class="sxs-lookup"><span data-stu-id="b18da-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="b18da-278">Добавьте следующие константы **ContactRepository** класс для представления имени для имени веб-сервера кэш элемент ключа далее в этом упражнении.</span><span class="sxs-lookup"><span data-stu-id="b18da-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="b18da-279">Добавьте конструктор для **ContactRepository** содержит следующий код.</span><span class="sxs-lookup"><span data-stu-id="b18da-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="b18da-280">(Фрагмент - кода *веб-конструктор репозитория контактов лаборатории API - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="b18da-281">Изменение кода для **GetAllContacts** метода, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b18da-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="b18da-282">(Фрагмент - кода *лаборатории API веб - Ex02 - получения всех контактов*)</span><span class="sxs-lookup"><span data-stu-id="b18da-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="b18da-283">В этом примере осуществляется для демонстрационных целей и будет использовать веб-сервер кэша как на носителе, чтобы значения будет доступен для нескольких клиентов одновременно, а не использовать механизм хранения данных сеанса или времени жизни запроса хранилища.</span><span class="sxs-lookup"><span data-stu-id="b18da-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="b18da-284">Можно было использовать Entity Framework, хранение XML-данных или любой другой разновидности вместо кэше веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="b18da-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="b18da-285">Реализовать новый метод с именем **SaveContact** для **ContactRepository** класса для выполнения операций сохранения контакта.</span><span class="sxs-lookup"><span data-stu-id="b18da-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="b18da-286">**SaveContact** метод должен принимать один **контакт** параметров и возвращаемых логическое значение, указывающее успешное выполнение или сбой.</span><span class="sxs-lookup"><span data-stu-id="b18da-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="b18da-287">(Фрагмент - кода *веб-API лаборатории - Ex02 - реализация метода SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="b18da-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="b18da-288">Упражнение 3: Использование веб-API из HTML-клиента</span><span class="sxs-lookup"><span data-stu-id="b18da-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="b18da-289">В этом упражнении вы создадите HTML-клиента для вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="b18da-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="b18da-290">Этот клиент облегчит обмена данными с веб-API, с помощью JavaScript и отображает результаты в веб-браузере, с помощью разметки HTML.</span><span class="sxs-lookup"><span data-stu-id="b18da-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="b18da-291">Задача 1 - Изменение представления индекса для обеспечения графический пользовательский Интерфейс для отображения контактов</span><span class="sxs-lookup"><span data-stu-id="b18da-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="b18da-292">В этой задаче предстоит изменить представление индекса по умолчанию веб-приложения для поддержки требование для отображения списка существующих контактов в HTML-браузером.</span><span class="sxs-lookup"><span data-stu-id="b18da-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="b18da-293">Откройте **Visual Studio 2012 Express для Web** , если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="b18da-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="b18da-294">Откройте **начать** решений, расположенный **источника/Ex03-ConsumingWebAPI/начало/** папки.</span><span class="sxs-lookup"><span data-stu-id="b18da-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="b18da-295">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="b18da-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b18da-296">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="b18da-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b18da-297">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b18da-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b18da-298">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="b18da-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b18da-299">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="b18da-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b18da-300">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b18da-301">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b18da-302">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="b18da-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b18da-303">Откройте **Index.cshtml** файл, расположенный в **представления/домашние** папки.</span><span class="sxs-lookup"><span data-stu-id="b18da-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="b18da-304">Замените код HTML в элемент div с идентификатором **текст** , чтобы он выглядел как следующий код.</span><span class="sxs-lookup"><span data-stu-id="b18da-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. <span data-ttu-id="b18da-305">Добавьте следующий код Javascript в нижней части файла для выполнения запроса HTTP для веб-API.</span><span class="sxs-lookup"><span data-stu-id="b18da-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. <span data-ttu-id="b18da-306">Откройте **ContactController.cs** файл, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="b18da-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="b18da-307">Установить точку останова на **получить** метод **ContactController** класса.</span><span class="sxs-lookup"><span data-stu-id="b18da-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="b18da-308">![Установив точку останова на метод Get контроллер API](build-restful-apis-with-aspnet-web-api/_static/image21.png "установив точку останова на метод Get контроллер API")</span><span class="sxs-lookup"><span data-stu-id="b18da-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="b18da-309">*Установив точку останова на метод Get контроллер API*</span><span class="sxs-lookup"><span data-stu-id="b18da-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="b18da-310">Нажмите клавишу **F5** для запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="b18da-310">Press **F5** to run the project.</span></span> <span data-ttu-id="b18da-311">Браузер будет загрузить документ HTML.</span><span class="sxs-lookup"><span data-stu-id="b18da-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b18da-312">Убедитесь, что при переходе корневой URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="b18da-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="b18da-313">После загрузки страницы и выполняет функции JavaScript, точка останова будет достигнута и выполнение кода будет приостановлено на контроллере.</span><span class="sxs-lookup"><span data-stu-id="b18da-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="b18da-314">![Отладка в вызовы веб-API, с помощью VS Express для Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "отладки в вызовы веб-API, с помощью VS Express для Web")</span><span class="sxs-lookup"><span data-stu-id="b18da-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="b18da-315">*Отладка в вызов веб-API, с помощью Visual Studio 2012 Express для Web*</span><span class="sxs-lookup"><span data-stu-id="b18da-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="b18da-316">Удаление точки останова и нажмите клавишу **F5** или панели инструментов отладки **Продолжить** кнопку, чтобы продолжить загрузку представление в браузере.</span><span class="sxs-lookup"><span data-stu-id="b18da-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="b18da-317">После завершения вызова веб-API вы увидите контактов, возвращенные веб-API вызовите отобразить в виде элементов списка в браузере.</span><span class="sxs-lookup"><span data-stu-id="b18da-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="b18da-318">![Результаты отображаются в браузере в виде элементов списка вызова API](build-restful-apis-with-aspnet-web-api/_static/image23.png "результаты вызова интерфейса API, которые отображаются в браузере в виде элементов списка")</span><span class="sxs-lookup"><span data-stu-id="b18da-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="b18da-319">*Результаты отображаются в браузере в виде элементов списка вызова интерфейса API*</span><span class="sxs-lookup"><span data-stu-id="b18da-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="b18da-320">Остановите отладку.</span><span class="sxs-lookup"><span data-stu-id="b18da-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="b18da-321">Задача 2 - изменение представления индекса обеспечивает графический пользовательский Интерфейс для создания контактов</span><span class="sxs-lookup"><span data-stu-id="b18da-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="b18da-322">В этой задаче будет продолжать изменения индекса представления MVC-приложения.</span><span class="sxs-lookup"><span data-stu-id="b18da-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="b18da-323">Формы, которые будут добавлены на HTML-страницу, будет ввод данных и отправить его в веб-API для создания нового контакта, и создается новый метод контроллера веб-API для сбора даты из графического интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b18da-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="b18da-324">Откройте **ContactController.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="b18da-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="b18da-325">Добавьте новый метод в класс контроллера с именем **Post** как показано в следующем коде.</span><span class="sxs-lookup"><span data-stu-id="b18da-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="b18da-326">(Фрагмент - кода *веб-метод Post лаборатории API - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="b18da-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. <span data-ttu-id="b18da-327">Откройте **Index.cshtml** файл в Visual Studio, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="b18da-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="b18da-328">Добавьте приведенный ниже код HTML в файл сразу после неупорядоченного списка, добавленного в предыдущей задаче.</span><span class="sxs-lookup"><span data-stu-id="b18da-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. <span data-ttu-id="b18da-329">В элементе скрипта в нижней части документа добавьте следующий выделенный код для обработки события нажатия кнопки, которые будут отправлены в данных веб-API с помощью вызова HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b18da-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="b18da-330">В **ContactController.cs**, разместить точки останова на **Post** метод.</span><span class="sxs-lookup"><span data-stu-id="b18da-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="b18da-331">Нажмите клавишу **F5** для запуска приложения в браузере.</span><span class="sxs-lookup"><span data-stu-id="b18da-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="b18da-332">После загрузки страницы в браузере введите имя нового контакта, идентификатора и щелкните **Сохранить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="b18da-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="b18da-333">![Загрузить документ клиента HTML в браузере](build-restful-apis-with-aspnet-web-api/_static/image24.png "клиентский HTML-документ загружен в браузере")</span><span class="sxs-lookup"><span data-stu-id="b18da-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="b18da-334">*Клиент HTML-документ загружен в браузере*</span><span class="sxs-lookup"><span data-stu-id="b18da-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="b18da-335">Когда в окне отладчика прерывается **Post** метода, принимающую рассматриваются свойства **обратитесь к** параметра.</span><span class="sxs-lookup"><span data-stu-id="b18da-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="b18da-336">Значения должны соответствовать данные, введенные в форме.</span><span class="sxs-lookup"><span data-stu-id="b18da-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="b18da-337">![Объект контакта, отправляемых с клиента веб-API](build-restful-apis-with-aspnet-web-api/_static/image25.png "объекта контакта, отправляемых с клиента веб-API")</span><span class="sxs-lookup"><span data-stu-id="b18da-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="b18da-338">*Объект контакта, отправляемых с клиента веб-API*</span><span class="sxs-lookup"><span data-stu-id="b18da-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="b18da-339">Пошаговое выполнение метода в отладчике до **ответ** создать переменную.</span><span class="sxs-lookup"><span data-stu-id="b18da-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="b18da-340">После проверки в **локальные** окна в отладчике, вы увидите что заданы все свойства.</span><span class="sxs-lookup"><span data-stu-id="b18da-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="b18da-341">![После создания в отладчике ответ](build-restful-apis-with-aspnet-web-api/_static/image26.png "ответ после создания в отладчике")</span><span class="sxs-lookup"><span data-stu-id="b18da-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="b18da-342">*Ответ после создания в отладчике*</span><span class="sxs-lookup"><span data-stu-id="b18da-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="b18da-343">Если нажать клавишу **F5** или нажмите кнопку **Продолжить** в отладчике запрос будет завершен.</span><span class="sxs-lookup"><span data-stu-id="b18da-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="b18da-344">При переключении обратно в браузер новый контакт добавлен к списку контактов, хранящихся в **ContactRepository** реализации.</span><span class="sxs-lookup"><span data-stu-id="b18da-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="b18da-345">![Браузер отражает успешного создания нового экземпляра, обратитесь в службу](build-restful-apis-with-aspnet-web-api/_static/image27.png "браузер отражает успешного создания нового экземпляра, обратитесь в службу")</span><span class="sxs-lookup"><span data-stu-id="b18da-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="b18da-346">*Браузер отражает успешного создания нового экземпляра, обратитесь в службу*</span><span class="sxs-lookup"><span data-stu-id="b18da-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="b18da-347">Кроме того, можно развернуть это приложение в Azure ниже [приложение C: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="b18da-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b18da-348">Сводка</span><span class="sxs-lookup"><span data-stu-id="b18da-348">Summary</span></span>

<span data-ttu-id="b18da-349">Эта лаборатория появился для новой платформы веб-API ASP.NET и реализации RESTful веб-API, с помощью платформы.</span><span class="sxs-lookup"><span data-stu-id="b18da-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="b18da-350">Здесь можно создать новый репозиторий, который упрощает сохранение данных с использованием любого числа механизмов и подключить эту службу, вместо простой тот, который указан в качестве примера в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="b18da-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="b18da-351">Веб-API поддерживает ряд дополнительных функций, таких как включение подключения от клиентов не на основе HTML, написанные на любом языке, поддерживающем HTTP и JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="b18da-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="b18da-352">Возможность размещения веб-API за пределами типичных веб-приложения можно также, а также является возможность создания собственных форматов сериализации.</span><span class="sxs-lookup"><span data-stu-id="b18da-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="b18da-353">Веб-сайт ASP.NET имеет область для .NET framework веб-API ASP.NET с [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="b18da-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="b18da-354">Этот сайт продолжить предоставлять последние сведения, примеры и новости, относящиеся к веб-API, поэтому он регулярно проверять Если вы хотите более подробно рассмотрим иллюстрации создания пользовательского веб-API для framework практически с любого устройства или для разработки.</span><span class="sxs-lookup"><span data-stu-id="b18da-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="b18da-355">Приложение а. Использование фрагментов кода</span><span class="sxs-lookup"><span data-stu-id="b18da-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="b18da-356">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="b18da-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b18da-357">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="b18da-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b18da-358">![Фрагменты кода Visual Studio для вставки кода в проекте](build-restful-apis-with-aspnet-web-api/_static/image28.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="b18da-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b18da-359">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="b18da-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="b18da-360">Добавление фрагмента кода, с помощью клавиатуры (только C#)</span><span class="sxs-lookup"><span data-stu-id="b18da-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="b18da-361">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="b18da-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b18da-362">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="b18da-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b18da-363">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="b18da-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b18da-364">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="b18da-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b18da-365">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="b18da-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="b18da-366">![Начните вводить имя фрагмента](build-restful-apis-with-aspnet-web-api/_static/image29.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="b18da-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="b18da-367">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="b18da-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="b18da-368">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](build-restful-apis-with-aspnet-web-api/_static/image30.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="b18da-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="b18da-369">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="b18da-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="b18da-370">![Снова нажмите клавишу Tab и фрагмент будет расширен](build-restful-apis-with-aspnet-web-api/_static/image31.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="b18da-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="b18da-371">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="b18da-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="b18da-372">Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)</span><span class="sxs-lookup"><span data-stu-id="b18da-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="b18da-373">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="b18da-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="b18da-374">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="b18da-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="b18da-375">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="b18da-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="b18da-376">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](build-restful-apis-with-aspnet-web-api/_static/image32.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="b18da-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="b18da-377">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="b18da-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="b18da-378">![Выберите из списка, соответствующего фрагмента, щелкнув его](build-restful-apis-with-aspnet-web-api/_static/image33.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="b18da-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="b18da-379">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="b18da-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b18da-380">Приложение б. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="b18da-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b18da-381">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b18da-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b18da-382">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b18da-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b18da-383">Последовательно выберите пункты [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b18da-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b18da-384">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b18da-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b18da-385">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="b18da-385">Click on **Install Now**.</span></span> <span data-ttu-id="b18da-386">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="b18da-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b18da-387">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="b18da-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b18da-388">![Установка Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b18da-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b18da-389">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b18da-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b18da-390">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="b18da-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="b18da-392">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="b18da-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b18da-393">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="b18da-393">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="b18da-395">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="b18da-395">*Installation progress*</span></span>
6. <span data-ttu-id="b18da-396">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="b18da-396">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="b18da-398">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="b18da-398">*Installation completed*</span></span>
7. <span data-ttu-id="b18da-399">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="b18da-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b18da-400">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="b18da-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="b18da-402">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="b18da-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b18da-403">Приложение в. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="b18da-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b18da-404">В этом приложении будет показано, как создать новый веб-сайт на портале Azure и публикация приложения, получить, выполнив в лаборатории преимуществами средство публикации веб-развертывания, предоставляемых Azure.</span><span class="sxs-lookup"><span data-stu-id="b18da-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="b18da-405">Задача 1. Создание нового веб-узла с помощью портала Azure</span><span class="sxs-lookup"><span data-stu-id="b18da-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="b18da-406">Последовательно выберите пункты [портала управления Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="b18da-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b18da-407">С помощью Azure можно бесплатно размещения 10 веб-сайтов ASP.NET и затем масштабирование по мере увеличения трафика.</span><span class="sxs-lookup"><span data-stu-id="b18da-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b18da-408">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b18da-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b18da-409">![Войдите на портал Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "входа на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b18da-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b18da-410">*Войдите на портал*</span><span class="sxs-lookup"><span data-stu-id="b18da-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="b18da-411">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="b18da-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b18da-412">![Создание нового веб-сайта](build-restful-apis-with-aspnet-web-api/_static/image40.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="b18da-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b18da-413">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b18da-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b18da-414">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="b18da-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b18da-415">Выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="b18da-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="b18da-416">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **Создание веб-сайта**.</span><span class="sxs-lookup"><span data-stu-id="b18da-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b18da-417">Azure размещается веб-приложение, работающее в облаке, вы можете управлять.</span><span class="sxs-lookup"><span data-stu-id="b18da-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b18da-418">Параметр быстрого создания позволяет развертывать завершенного веб-приложения в Azure из вне портала.</span><span class="sxs-lookup"><span data-stu-id="b18da-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="b18da-419">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="b18da-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b18da-420">![Создание нового веб-сайта с помощью быстрого создания](build-restful-apis-with-aspnet-web-api/_static/image41.png "создания нового веб-сайта с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="b18da-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b18da-421">*Создание нового веб-сайта с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="b18da-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b18da-422">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="b18da-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b18da-423">После создания веб-сайта по ссылке **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="b18da-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b18da-424">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="b18da-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b18da-425">![Просмотр новых веб-сайт](build-restful-apis-with-aspnet-web-api/_static/image42.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="b18da-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b18da-426">*Просмотр новых веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="b18da-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b18da-427">![Веб-сайта под управлением](build-restful-apis-with-aspnet-web-api/_static/image43.png "веб-сайта под управлением")</span><span class="sxs-lookup"><span data-stu-id="b18da-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="b18da-428">*Запуск веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b18da-428">*Web site running*</span></span>
6. <span data-ttu-id="b18da-429">Вернитесь на портал и щелкните имя веб-сайта в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="b18da-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b18da-430">![Открытие страницы управления веб-сайт](build-restful-apis-with-aspnet-web-api/_static/image44.png "Открытие страниц управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="b18da-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b18da-431">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b18da-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b18da-432">В **панели мониторинга** в разделе **беглого** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="b18da-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b18da-433">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения в Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="b18da-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="b18da-434">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверки подлинности с каждой из конечных точек, для которых включен метода публикации.</span><span class="sxs-lookup"><span data-stu-id="b18da-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b18da-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки этих программ для Публикация веб-приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="b18da-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="b18da-436">![Профиль публикации веб-сайт загрузки](build-restful-apis-with-aspnet-web-api/_static/image45.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="b18da-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b18da-437">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="b18da-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b18da-438">Загрузите файл профиля публикации в известном расположении.</span><span class="sxs-lookup"><span data-stu-id="b18da-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b18da-439">Далее в этом упражнении вы увидите как использовать этот файл для публикации веб-приложения в Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b18da-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="b18da-440">![Сохранить файл профиля публикации](build-restful-apis-with-aspnet-web-api/_static/image46.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="b18da-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b18da-441">*Сохранить файл профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="b18da-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b18da-442">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="b18da-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b18da-443">Если приложение использует SQL Server базы данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="b18da-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b18da-444">Если вы хотите развернуть простое приложение, которое не использует SQL Server может пропустить эту задачу.</span><span class="sxs-lookup"><span data-stu-id="b18da-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b18da-445">Сервер базы данных SQL необходимо для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="b18da-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b18da-446">Можно просматривать серверы базы данных SQL из подписки на портале управления Azure на **баз данных Sql** | **серверы** | **мониторинга сервера**.</span><span class="sxs-lookup"><span data-stu-id="b18da-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b18da-447">Если у вас на сервер, созданный, можно создать ее, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="b18da-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b18da-448">Обратите внимание **имя сервера и URL-адрес, имя входа администратора и пароль**, как будет использовать их в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="b18da-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b18da-449">Не следует создавать базу данных, как он будет создан в более поздней стадии.</span><span class="sxs-lookup"><span data-stu-id="b18da-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b18da-450">![Панель мониторинга базы данных SQL Server](build-restful-apis-with-aspnet-web-api/_static/image47.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="b18da-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b18da-451">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="b18da-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b18da-452">В следующей задаче требуется проверить подключение к базе данных из Visual Studio, по этой причине необходимо включить локальный IP-адреса в список серверов **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="b18da-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b18da-453">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="b18da-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Добавление IP-адрес клиента](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="b18da-455">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="b18da-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b18da-456">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="b18da-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="b18da-458">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="b18da-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b18da-459">Задача 3. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="b18da-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b18da-460">Вернитесь в решении ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b18da-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b18da-461">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="b18da-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b18da-462">![Публикация приложения](build-restful-apis-with-aspnet-web-api/_static/image51.png "публикация приложения")</span><span class="sxs-lookup"><span data-stu-id="b18da-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="b18da-463">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="b18da-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="b18da-464">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="b18da-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b18da-465">![Импорт профиля публикации](build-restful-apis-with-aspnet-web-api/_static/image52.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="b18da-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b18da-466">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="b18da-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="b18da-467">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="b18da-467">Click **Validate Connection**.</span></span> <span data-ttu-id="b18da-468">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b18da-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b18da-469">Проверка завершена, как только вы увидите зеленый флажок рядом с "Проверить подключение".</span><span class="sxs-lookup"><span data-stu-id="b18da-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b18da-470">![Проверка подключения](build-restful-apis-with-aspnet-web-api/_static/image53.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="b18da-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="b18da-471">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="b18da-471">*Validating connection*</span></span>
4. <span data-ttu-id="b18da-472">В **параметры** в разделе **баз данных** статьи, нажмите кнопку рядом с текстового поля для подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b18da-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b18da-473">![Конфигурация веб-развертывания](build-restful-apis-with-aspnet-web-api/_static/image54.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="b18da-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b18da-474">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="b18da-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b18da-475">Настройте подключение к базе данных следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b18da-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b18da-476">В **имя сервера** введите ваш URL-адрес сервера базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="b18da-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b18da-477">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="b18da-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b18da-478">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="b18da-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b18da-479">Введите имя новой базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="b18da-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="b18da-480">![Настройка строки подключения назначения](build-restful-apis-with-aspnet-web-api/_static/image55.png "Настройка целевой строки подключения")</span><span class="sxs-lookup"><span data-stu-id="b18da-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b18da-481">*Настройка целевой строки подключения*</span><span class="sxs-lookup"><span data-stu-id="b18da-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b18da-482">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b18da-482">Then click **OK**.</span></span> <span data-ttu-id="b18da-483">Когда появится запрос на создание базы данных щелкните **Да**.</span><span class="sxs-lookup"><span data-stu-id="b18da-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b18da-484">![Создание базы данных](build-restful-apis-with-aspnet-web-api/_static/image56.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="b18da-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="b18da-485">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="b18da-485">*Creating the database*</span></span>
7. <span data-ttu-id="b18da-486">Строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure отображается внутри текстового поля по умолчанию соединения.</span><span class="sxs-lookup"><span data-stu-id="b18da-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b18da-487">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b18da-487">Then click **Next**.</span></span>

    <span data-ttu-id="b18da-488">![Строка подключения, указывающий базу данных SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "строку соединения, указывающий базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="b18da-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b18da-489">*Строка подключения, указывающий базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="b18da-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b18da-490">В **предварительного просмотра** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="b18da-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b18da-491">![Публикация веб-приложения](build-restful-apis-with-aspnet-web-api/_static/image58.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="b18da-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="b18da-492">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="b18da-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="b18da-493">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-узла.</span><span class="sxs-lookup"><span data-stu-id="b18da-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="b18da-494">![Публикации приложения в Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b18da-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="b18da-495">*Приложения, опубликованные в Azure*</span><span class="sxs-lookup"><span data-stu-id="b18da-495">*Application published to Azure*</span></span>
