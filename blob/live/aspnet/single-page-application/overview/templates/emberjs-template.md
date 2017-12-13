---
uid: single-page-application/overview/templates/emberjs-template
title: "Шаблон EmberJS | Документы Microsoft"
author: xqiu
description: "Шаблон EmberJS"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a><span data-ttu-id="c5372-103">Шаблон EmberJS</span><span class="sxs-lookup"><span data-stu-id="c5372-103">EmberJS template</span></span>
====================
<span data-ttu-id="c5372-104">по [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="c5372-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="c5372-105">Шаблон MVC EmberJS записывается Натана Тоттена, Тиаго Сантос и Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="c5372-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="c5372-106">Загрузите шаблон EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="c5372-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="c5372-107">Чтобы приступить к работе быстрого создания интерактивные клиентские веб-приложения с помощью EmberJS предназначен шаблон EmberJS SPA.</span><span class="sxs-lookup"><span data-stu-id="c5372-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="c5372-108">«Одностраничного приложения» (SPA) — это общий термин для веб-приложения, которое загружает одной HTML-страницы и затем обновляет страницу динамически, вместо загрузки новой страницы.</span><span class="sxs-lookup"><span data-stu-id="c5372-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="c5372-109">После загрузки начальной страницы SPA обращается к серверу через запросы AJAX.</span><span class="sxs-lookup"><span data-stu-id="c5372-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="c5372-110">AJAX нет ничего нового, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание больших сложных приложений SPA.</span><span class="sxs-lookup"><span data-stu-id="c5372-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="c5372-111">Кроме того HTML 5 и CSS3 упрощая процесс для создания многофункциональных пользовательских интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="c5372-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="c5372-112">Использует шаблон SPA EmberJS [Ember](http://emberjs.com/) библиотека JavaScript для обработки обновлений страницы из запросы AJAX.</span><span class="sxs-lookup"><span data-stu-id="c5372-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="c5372-113">Ember.js использует привязку данных для синхронизации на странице с использованием последних данных.</span><span class="sxs-lookup"><span data-stu-id="c5372-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="c5372-114">В этом случае не нужно писать код, который описывается данным JSON и обновляет модели DOM.</span><span class="sxs-lookup"><span data-stu-id="c5372-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="c5372-115">Вместо этого вы поместите декларативные атрибуты в формате HTML, сообщить Ember.js способ представления данных.</span><span class="sxs-lookup"><span data-stu-id="c5372-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="c5372-116">На стороне сервера шаблона EmberJS практически идентичен [шаблона с использованием KnockoutJS SPA](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="c5372-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="c5372-117">ASP.NET MVC используется для работы с HTML-документов и веб-API ASP.NET для обработки запросов AJAX от клиента.</span><span class="sxs-lookup"><span data-stu-id="c5372-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="c5372-118">Дополнительные сведения об этих аспектов шаблона посвящены [шаблона с использованием KnockoutJS](../introduction/knockoutjs-template.md) документации.</span><span class="sxs-lookup"><span data-stu-id="c5372-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="c5372-119">Этот раздел посвящен различия между Knockout и EmberJS шаблона.</span><span class="sxs-lookup"><span data-stu-id="c5372-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="c5372-120">Создайте проект шаблона EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="c5372-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="c5372-121">Загрузите и установите шаблон, нажав кнопку «Загрузить» выше.</span><span class="sxs-lookup"><span data-stu-id="c5372-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="c5372-122">Может потребоваться перезапустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5372-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="c5372-123">В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла.</span><span class="sxs-lookup"><span data-stu-id="c5372-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c5372-124">В разделе **Visual C#**выберите **Web**.</span><span class="sxs-lookup"><span data-stu-id="c5372-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c5372-125">В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="c5372-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c5372-126">Имя проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c5372-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="c5372-127">В **новый проект** мастера выберите **Ember.js SPA проекта**.</span><span class="sxs-lookup"><span data-stu-id="c5372-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="c5372-128">Обзор шаблона EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="c5372-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="c5372-129">Шаблон EmberJS использует сочетание jQuery, Ember.js, Handlebars.js создать smooth, интерактивный пользовательский Интерфейс.</span><span class="sxs-lookup"><span data-stu-id="c5372-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="c5372-130">Ember.js — это библиотека JavaScript, которая использует шаблон MVC клиентские.</span><span class="sxs-lookup"><span data-stu-id="c5372-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="c5372-131">Объект *шаблона*, написанный на языке шаблонов рули описывает пользовательский интерфейс приложения.</span><span class="sxs-lookup"><span data-stu-id="c5372-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="c5372-132">В режиме выпуска [компилятора рули](https://github.com/Myslik/csharp-ember-handlebars) используется для объединения и скомпилируйте рули шаблона.</span><span class="sxs-lookup"><span data-stu-id="c5372-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="c5372-133">Объект *модель* хранит данные приложений, которые он получает от сервера (ToDo списки и элементы ToDo).</span><span class="sxs-lookup"><span data-stu-id="c5372-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="c5372-134">Объект *контроллера* хранит состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="c5372-134">A *controller* stores application state.</span></span> <span data-ttu-id="c5372-135">Контроллеры часто представляют данные модели, соответствующие шаблоны.</span><span class="sxs-lookup"><span data-stu-id="c5372-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="c5372-136">Объект *представление* преобразует простых событий приложения и передает их на контроллер.</span><span class="sxs-lookup"><span data-stu-id="c5372-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="c5372-137">Объект *маршрутизатора* управляет состоянием приложения, синхронизируя URL-адреса и шаблоны.</span><span class="sxs-lookup"><span data-stu-id="c5372-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="c5372-138">Кроме того библиотека Ember данных может использоваться для синхронизации объектов JSON (полученный с сервера через RESTful API) и клиентские модели.</span><span class="sxs-lookup"><span data-stu-id="c5372-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="c5372-139">Шаблон EmberJS SPA скрипты организованы по восемь слоев:</span><span class="sxs-lookup"><span data-stu-id="c5372-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="c5372-140">webapi\_adapter.js, webapi\_serializer.js: расширяет библиотеку Ember данных для работы с веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c5372-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="c5372-141">Scripts/Helpers.js: Определяет новый Ember рули вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="c5372-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="c5372-142">Scripts/App.js: Приложение создает и настраивает адаптер и сериализатор.</span><span class="sxs-lookup"><span data-stu-id="c5372-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="c5372-143">Скрипты, модели иприложений/\*.js: Определяет моделей.</span><span class="sxs-lookup"><span data-stu-id="c5372-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="c5372-144">Сценарии иприложения/представления/\*.js: Определяет представления.</span><span class="sxs-lookup"><span data-stu-id="c5372-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="c5372-145">Скрипты, приложения или контроллеров или\*.js: определяет контроллеры.</span><span class="sxs-lookup"><span data-stu-id="c5372-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="c5372-146">Скрипты, приложения или маршруты, Scripts/app/router.js: Определяет маршруты.</span><span class="sxs-lookup"><span data-stu-id="c5372-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="c5372-147">Шаблоны и\*.hbs: Определяет рули шаблоны.</span><span class="sxs-lookup"><span data-stu-id="c5372-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="c5372-148">Рассмотрим некоторые из этих сценариев более подробно.</span><span class="sxs-lookup"><span data-stu-id="c5372-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="c5372-149">Модели</span><span class="sxs-lookup"><span data-stu-id="c5372-149">Models</span></span>

<span data-ttu-id="c5372-150">Модели определяются в папке скриптов, модели или приложения.</span><span class="sxs-lookup"><span data-stu-id="c5372-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="c5372-151">Существуют два файла модели: todoItem.js и todoList.js.</span><span class="sxs-lookup"><span data-stu-id="c5372-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="c5372-152">**TODO.Model.js** определяет модели (браузер) на стороне клиента для списков дел.</span><span class="sxs-lookup"><span data-stu-id="c5372-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="c5372-153">Существует два класса модели: todoItem и todoList.</span><span class="sxs-lookup"><span data-stu-id="c5372-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="c5372-154">В Ember моделей являются подклассами доменных служб Active Directory. Модель.</span><span class="sxs-lookup"><span data-stu-id="c5372-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="c5372-155">Модель может иметь свойства, атрибуты которых:</span><span class="sxs-lookup"><span data-stu-id="c5372-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="c5372-156">Модели можно определить отношения с другими моделями:</span><span class="sxs-lookup"><span data-stu-id="c5372-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="c5372-157">Моделей может вычислено свойства, которые привязаны к другим свойствам:</span><span class="sxs-lookup"><span data-stu-id="c5372-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="c5372-158">Модели может иметь наблюдателя функций, которые вызываются при изменении свойства наблюдаемых:</span><span class="sxs-lookup"><span data-stu-id="c5372-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="c5372-159">Представления</span><span class="sxs-lookup"><span data-stu-id="c5372-159">Views</span></span>

<span data-ttu-id="c5372-160">Представления определяются в папке скриптов, приложений или представления.</span><span class="sxs-lookup"><span data-stu-id="c5372-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="c5372-161">Представление преобразует события из пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="c5372-161">A view translates events from the application UI.</span></span> <span data-ttu-id="c5372-162">Обработчик событий может обратный вызов функции контроллера или просто напрямую вызвать контекста данных.</span><span class="sxs-lookup"><span data-stu-id="c5372-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="c5372-163">Например приведенный ниже код взят из views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="c5372-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="c5372-164">Он определяет обработки текстовое поле ввода.</span><span class="sxs-lookup"><span data-stu-id="c5372-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="c5372-165">Контроллер</span><span class="sxs-lookup"><span data-stu-id="c5372-165">Controller</span></span>

<span data-ttu-id="c5372-166">Контроллеры определяются в папке скриптов, приложений или контроллеров.</span><span class="sxs-lookup"><span data-stu-id="c5372-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="c5372-167">Для представления одной модели, расширить `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="c5372-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="c5372-168">Контроллер может также представлять собой коллекцию моделей, расширяя `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="c5372-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="c5372-169">Например, TodoListController представляет собой массив `todoList` объектов.</span><span class="sxs-lookup"><span data-stu-id="c5372-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="c5372-170">Контроллер сортирует по Идентификатору todoList, в убывающем порядке:</span><span class="sxs-lookup"><span data-stu-id="c5372-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="c5372-171">Контроллер определяет функцию с именем `addTodoList`, который создает новый todoList и добавляет его в массив.</span><span class="sxs-lookup"><span data-stu-id="c5372-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="c5372-172">Чтобы увидеть, как вызывается эта функция, откройте файл шаблона с именем todoListTemplate.html в папке «Шаблоны».</span><span class="sxs-lookup"><span data-stu-id="c5372-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="c5372-173">Следующий код шаблона привязывает кнопку, чтобы `addTodoList` функции:</span><span class="sxs-lookup"><span data-stu-id="c5372-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="c5372-174">Контроллер также содержит `error` свойство, которое содержит сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="c5372-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="c5372-175">Ниже приведен код шаблона, чтобы показать сообщение об ошибке (а также в todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="c5372-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="c5372-176">Маршруты</span><span class="sxs-lookup"><span data-stu-id="c5372-176">Routes</span></span>

<span data-ttu-id="c5372-177">Router.js определяет маршруты и шаблон по умолчанию для отображения настраивает состояния приложения и сопоставляет URL-адреса маршруты:</span><span class="sxs-lookup"><span data-stu-id="c5372-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="c5372-178">TodoListRoute.js загружает данные для TodoListRoute путем переопределения setupController функции:</span><span class="sxs-lookup"><span data-stu-id="c5372-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="c5372-179">Ember использует соглашения об именовании для соответствия URL-адреса, имена маршрутов, контроллеров и шаблоны.</span><span class="sxs-lookup"><span data-stu-id="c5372-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="c5372-180">Дополнительные сведения см. в разделе [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS документацию.</span><span class="sxs-lookup"><span data-stu-id="c5372-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="c5372-181">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="c5372-181">Templates</span></span>

<span data-ttu-id="c5372-182">Папка шаблонов содержит четыре шаблона:</span><span class="sxs-lookup"><span data-stu-id="c5372-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="c5372-183">Application.hbs: шаблон по умолчанию, который отображается при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="c5372-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="c5372-184">About.hbs: шаблона маршрута «/ о программе».</span><span class="sxs-lookup"><span data-stu-id="c5372-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="c5372-185">index.hbs: шаблон для корня маршрута «/».</span><span class="sxs-lookup"><span data-stu-id="c5372-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="c5372-186">todoList.hbs: шаблона для «/ todo» маршрута.</span><span class="sxs-lookup"><span data-stu-id="c5372-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="c5372-187">\_NavBar.hbs: шаблон определяет меню навигации.</span><span class="sxs-lookup"><span data-stu-id="c5372-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="c5372-188">Шаблон приложения ведет себя как главной страницы.</span><span class="sxs-lookup"><span data-stu-id="c5372-188">The application template acts like a master page.</span></span> <span data-ttu-id="c5372-189">Он содержит заголовок, нижний колонтитул и «{{розетки}}» для вставки другие шаблоны, в зависимости от маршрута.</span><span class="sxs-lookup"><span data-stu-id="c5372-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="c5372-190">Дополнительные сведения о шаблонах приложений в Ember см. в разделе [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="c5372-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="c5372-191">«/ TodoList» шаблон содержит два выражения цикла.</span><span class="sxs-lookup"><span data-stu-id="c5372-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="c5372-192">Находится вне цикла `{{#each controller}}`и внутренние цикл `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="c5372-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="c5372-193">В следующем коде показано встроенная `Ember.Checkbox` просмотреть, настраиваемый `App.TodoItemEditView`и компоновку с `deleteTodo` действие.</span><span class="sxs-lookup"><span data-stu-id="c5372-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="c5372-194">`HtmlHelperExtensions` Класс, определенный в Controllers/HtmlHelperExensions.cs, определяет вспомогательный класс функции кэширования и вставить шаблон файлов при **отладки** равно **true** в файле Web.config.</span><span class="sxs-lookup"><span data-stu-id="c5372-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="c5372-195">Эта функция вызывается из файла представления ASP.NET MVC, определенные в Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="c5372-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="c5372-196">Вызван без аргументов, функция отображает все файлы шаблонов в папке «Шаблоны».</span><span class="sxs-lookup"><span data-stu-id="c5372-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="c5372-197">Можно также указать вложенную папку или файл определенного шаблона.</span><span class="sxs-lookup"><span data-stu-id="c5372-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="c5372-198">Когда **отладки** — **false** в файле Web.config, приложение включает элемент набора «~/bundles/templates».</span><span class="sxs-lookup"><span data-stu-id="c5372-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="c5372-199">Этот элемент в пакет добавляется в BundleConfig.cs, с помощью компилятора рули библиотеки:</span><span class="sxs-lookup"><span data-stu-id="c5372-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
