---
title: "С помощью AngularJS для приложений на одной странице (SPAs)"
author: rick-anderson
description: "Сведения о создании приложения ASP.NET SPA стиля, с помощью AngularJS"
keywords: ASP.NET Core, AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4aecf9e9bd11cc7e2b36b40955178d9e9368c185
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="2fb92-104">С помощью AngularJS для приложений на одной странице (SPAs) с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fb92-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="2fb92-105">По [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2fb92-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="2fb92-106">В этой статье вы узнаете, как создать приложение ASP.NET SPA стиля, с помощью AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="2fb92-107">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2fb92-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="2fb92-108">Что такое AngularJS?</span><span class="sxs-lookup"><span data-stu-id="2fb92-108">What is AngularJS?</span></span>

<span data-ttu-id="2fb92-109">[AngularJS](https://angularjs.org/) — это современная платформа JavaScript из Google, обычно используется для работы с приложениями одной страницы (SPAs).</span><span class="sxs-lookup"><span data-stu-id="2fb92-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="2fb92-110">AngularJS открытой источником в рамках лицензии MIT и может располагаться после разработки ход AngularJS [своего репозитория GitHub](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="2fb92-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="2fb92-111">Библиотеке называется угловая, поскольку HTML использует углового были сформированы квадратные скобки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="2fb92-112">AngularJS не является библиотекой манипуляции DOM, например jQuery, но она использует подмножество называется jQLite jQuery.</span><span class="sxs-lookup"><span data-stu-id="2fb92-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="2fb92-113">AngularJS в основе декларативных атрибутов HTML, которые можно добавить теги HTML.</span><span class="sxs-lookup"><span data-stu-id="2fb92-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="2fb92-114">Можно попробовать AngularJS в браузере с помощью [School код веб-сайта](https://www.codeschool.com/courses/shaping-up-with-angularjs) или [W3Schools веб-сайт](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="2fb92-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="2fb92-115">Эта статья посвящена AngularJS некоторые особенности, на котором заголовок угловая.</span><span class="sxs-lookup"><span data-stu-id="2fb92-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2fb92-116">Начало работы</span><span class="sxs-lookup"><span data-stu-id="2fb92-116">Getting started</span></span>

<span data-ttu-id="2fb92-117">Чтобы начать использовать AngularJS в приложении ASP.NET, необходимо установить его как часть проекта или сослаться на нее из сети доставки содержимого (CDN).</span><span class="sxs-lookup"><span data-stu-id="2fb92-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="2fb92-118">Установка</span><span class="sxs-lookup"><span data-stu-id="2fb92-118">Installation</span></span>

<span data-ttu-id="2fb92-119">Добавление AngularJS в приложении несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="2fb92-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="2fb92-120">Если вы начинаете новое веб-приложение ASP.NET Core в Visual Studio, можно добавить с помощью встроенной AngularJS [Bower](bower.md) поддержки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="2fb92-121">Откройте *bower.json*и добавить запись `dependencies` свойство:</span><span class="sxs-lookup"><span data-stu-id="2fb92-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="2fb92-122">При сохранении *bower.json* файл, угловая устанавливается в своем проекте *wwwroot/lib* папки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="2fb92-123">Кроме того, оно будет отображаться в `Dependencies/Bower` папки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="2fb92-124">См. на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="2fb92-124">See the screenshot below.</span></span>

![Обозреватель решений с проектом AngularJS](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="2fb92-126">Добавьте `<script>` ссылку внизу `<body>` части HTML-страницы или *_Layout.cshtml* файла, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="2fb92-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="2fb92-127">Рекомендуется, что производственных приложений использовать CDN для стандартных библиотек, как AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="2fb92-128">AngularJS можно ссылаться из одного из нескольких CDN, подобные следующему:</span><span class="sxs-lookup"><span data-stu-id="2fb92-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="2fb92-129">Получив ссылку на *angular.js* файл скрипта, вы будете готовы приступить к использованию AngularJS на веб-страницах.</span><span class="sxs-lookup"><span data-stu-id="2fb92-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="2fb92-130">Основные компоненты</span><span class="sxs-lookup"><span data-stu-id="2fb92-130">Key components</span></span>

<span data-ttu-id="2fb92-131">AngularJS включает ряд основных компонентов, таких как *директивы*, *шаблоны*, *знаки повторения*, *модули*,  *контроллеры*, *компоненты*, *маршрутизатора компонент* и многое другое.</span><span class="sxs-lookup"><span data-stu-id="2fb92-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="2fb92-132">Давайте рассмотрим, как эти компоненты взаимодействуют для добавления поведения на веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="2fb92-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="2fb92-133">Директивы</span><span class="sxs-lookup"><span data-stu-id="2fb92-133">Directives</span></span>

<span data-ttu-id="2fb92-134">Использует AngularJS [директивы](https://docs.angularjs.org/guide/directive) расширить HTML с пользовательскими атрибутами и элементами.</span><span class="sxs-lookup"><span data-stu-id="2fb92-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="2fb92-135">Директивы AngularJS определяются через `data-ng-*` или `ng-*` префиксы (`ng` — сокращение угловое).</span><span class="sxs-lookup"><span data-stu-id="2fb92-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="2fb92-136">Существует два вида директивы AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="2fb92-137">**Примитив директивы**: эти стандартные углового командой и являются частью платформы AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="2fb92-138">**Пользовательские директивы**: это пользовательских директив, которые можно определить.</span><span class="sxs-lookup"><span data-stu-id="2fb92-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="2fb92-139">Одним из простых директив, используемых во всех приложениях AngularJS является `ng-app` директивы, который загружает приложения AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="2fb92-140">Эта директива может применяться к `<body>` тег или дочерний элемент текста.</span><span class="sxs-lookup"><span data-stu-id="2fb92-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="2fb92-141">Рассмотрим пример, в действии.</span><span class="sxs-lookup"><span data-stu-id="2fb92-141">Let's see an example in action.</span></span> <span data-ttu-id="2fb92-142">Если вы находитесь в проекте ASP.NET, можно либо добавить HTML-файл для `wwwroot` папки, или добавьте новое действие контроллера и связанного представления.</span><span class="sxs-lookup"><span data-stu-id="2fb92-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="2fb92-143">В этом случае я добавил новый `Directives` методом действия `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="2fb92-144">Здесь показан связанного представления:</span><span class="sxs-lookup"><span data-stu-id="2fb92-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="2fb92-145">Чтобы эти образцы независимы друг от друга, не используется файл общего макета.</span><span class="sxs-lookup"><span data-stu-id="2fb92-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="2fb92-146">Вы увидите, что мы внутренние тега body с `ng-app` директиву, чтобы указать на этой странице — это приложение AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="2fb92-147">`{{2+2}}` Выражение привязки углового данных, которое будет рассмотрено более чуть позже.</span><span class="sxs-lookup"><span data-stu-id="2fb92-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="2fb92-148">Ниже приведен результат, если вы запустите это приложение:</span><span class="sxs-lookup"><span data-stu-id="2fb92-148">Here is the result if you run this application:</span></span>

![Простой угловой директивы](angular/_static/simple-directive.png)

<span data-ttu-id="2fb92-150">Другие директивы примитивы в AngularJS включают:</span><span class="sxs-lookup"><span data-stu-id="2fb92-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="2fb92-151">`ng-controller`Определяет, какой контроллер JavaScript привязан к какой вид.</span><span class="sxs-lookup"><span data-stu-id="2fb92-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="2fb92-152">`ng-model`Определяет модель, с которыми связаны значения свойств элемента HTML.</span><span class="sxs-lookup"><span data-stu-id="2fb92-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="2fb92-153">`ng-init`Используется для инициализации данных приложения в виде выражения для текущей области.</span><span class="sxs-lookup"><span data-stu-id="2fb92-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="2fb92-154">`ng-if`Удаляет и повторно создает HTML-элемента, заданного в модели DOM, в зависимости от truthiness введенное выражение.</span><span class="sxs-lookup"><span data-stu-id="2fb92-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="2fb92-155">`ng-repeat`Повторяет данного блока HTML для набора данных.</span><span class="sxs-lookup"><span data-stu-id="2fb92-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="2fb92-156">`ng-show`Показывает или скрывает данного элемента HTML на основе выражения, предоставленные.</span><span class="sxs-lookup"><span data-stu-id="2fb92-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="2fb92-157">Полный список всех примитивов директив, поддерживаемые в AngularJS, можно найти [разделе директив документации на веб-сайте AngularJS документации](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="2fb92-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="2fb92-158">привязка данных,</span><span class="sxs-lookup"><span data-stu-id="2fb92-158">Data binding</span></span>

<span data-ttu-id="2fb92-159">Предоставляет AngularJS [привязки данных](https://docs.angularjs.org/guide/databinding) поддерживает out-of--box с помощью `ng-bind` директива или данные, такие как синтаксис выражений привязки `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="2fb92-160">AngularJS поддерживает двустороннюю привязку данных хранения данных из модели в синхронизации с помощью шаблона представления в любое время.</span><span class="sxs-lookup"><span data-stu-id="2fb92-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="2fb92-161">Любые изменения в представлении автоматически отражаются в модели.</span><span class="sxs-lookup"><span data-stu-id="2fb92-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="2fb92-162">Аналогичным образом любые изменения в модели, отражаются в представлении.</span><span class="sxs-lookup"><span data-stu-id="2fb92-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="2fb92-163">Создать HTML-файл или действия контроллера с сопутствующей представлением с именем `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="2fb92-164">Следующие представления:</span><span class="sxs-lookup"><span data-stu-id="2fb92-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="2fb92-165">Обратите внимание, что можно отобразить значения модели, с помощью директивы или данные привязки (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="2fb92-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="2fb92-166">Получившаяся в результате страница должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2fb92-166">The resulting page should look like this:</span></span>

![Простая привязка данных](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="2fb92-168">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="2fb92-168">Templates</span></span>

<span data-ttu-id="2fb92-169">[Шаблоны](https://docs.angularjs.org/guide/templates) в AngularJS просто HTML-страниц оформляются с директивы AngularJS и артефактов.</span><span class="sxs-lookup"><span data-stu-id="2fb92-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="2fb92-170">Шаблон AngularJS находится несколько директив, выражений, фильтров и элементы управления, объединяющие с кодом HTML, представление формы.</span><span class="sxs-lookup"><span data-stu-id="2fb92-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="2fb92-171">Добавление другого представления для демонстрации шаблонов и добавьте в него следующий:</span><span class="sxs-lookup"><span data-stu-id="2fb92-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="2fb92-172">Шаблон содержит директивы AngularJS как `ng-app`, `ng-init`, `ng-model` и синтаксис выражений привязки данных для привязки `personName` свойство.</span><span class="sxs-lookup"><span data-stu-id="2fb92-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="2fb92-173">Выполняется в браузере, представление выглядит следующим образом на снимке экрана ниже.</span><span class="sxs-lookup"><span data-stu-id="2fb92-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Простой пример шаблоны 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="2fb92-175">Если изменить имя, введя в поле ввода, вы увидите текст рядом с полем ввода динамическое обновление, показывающая углового двухстороннюю привязку данных в действие.</span><span class="sxs-lookup"><span data-stu-id="2fb92-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Простой пример шаблоны 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="2fb92-177">Выражения</span><span class="sxs-lookup"><span data-stu-id="2fb92-177">Expressions</span></span>

<span data-ttu-id="2fb92-178">[Выражения](https://docs.angularjs.org/guide/expression) в AngularJS — это фрагменты кода JavaScript, которые записываются в `{{ expression }}` синтаксиса.</span><span class="sxs-lookup"><span data-stu-id="2fb92-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="2fb92-179">Данные из этих выражений привязан к HTML так же, как `ng-bind` директивы.</span><span class="sxs-lookup"><span data-stu-id="2fb92-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="2fb92-180">Основное различие между AngularJS выражений и регулярных выражений JavaScript является этой AngularJS выражений производится по `$scope` объекта в AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="2fb92-181">AngularJS выражения в примере ниже привязки `personName` и простой JavaScript вычисляется выражение:</span><span class="sxs-lookup"><span data-stu-id="2fb92-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="2fb92-182">Пример, в браузере отображается `personName` данных и результаты вычисления:</span><span class="sxs-lookup"><span data-stu-id="2fb92-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Простые выражения](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="2fb92-184">Знаки повторения</span><span class="sxs-lookup"><span data-stu-id="2fb92-184">Repeaters</span></span>

<span data-ttu-id="2fb92-185">Повторять в AngularJS выполняется через простые директива вызывается `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="2fb92-186">`ng-repeat` Директива повторяет данного элемента HTML в представлении по длине массива повторяющихся данных.</span><span class="sxs-lookup"><span data-stu-id="2fb92-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="2fb92-187">Знаки повторения в AngularJS можно повторить к массиву строк или объектов.</span><span class="sxs-lookup"><span data-stu-id="2fb92-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="2fb92-188">Ниже приведен пример использования повторяющихся к массиву строк.</span><span class="sxs-lookup"><span data-stu-id="2fb92-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="2fb92-189">[Repeat-директива](https://docs.angularjs.org/api/ng/directive/ngRepeat) выводит ряд элементов списка в неупорядоченный список, как видно в средствах разработчика на этом снимке экрана показано:</span><span class="sxs-lookup"><span data-stu-id="2fb92-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Пример повторителя](angular/_static/repeater.png)

<span data-ttu-id="2fb92-191">Ниже приведен пример, в котором повторяется к массиву объектов.</span><span class="sxs-lookup"><span data-stu-id="2fb92-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="2fb92-192">`ng-init` Устанавливает директива `names` массива, где каждый элемент является объектом, содержащим сначала имени и фамилии.</span><span class="sxs-lookup"><span data-stu-id="2fb92-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="2fb92-193">`ng-repeat` Назначения, `name in names`, выводит элемент списка для каждого элемента массива.</span><span class="sxs-lookup"><span data-stu-id="2fb92-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="2fb92-194">Выходные данные в этом случае является таким же, как в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="2fb92-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="2fb92-195">Угловая предоставляет некоторые дополнительные директивы, которые могут помочь в составлении поведение в зависимости от того, где цикла в ходе выполнения.</span><span class="sxs-lookup"><span data-stu-id="2fb92-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="2fb92-196">Используйте `$index` в `ng-repeat` цикла, чтобы определить, какой индекс позиции в цикле в настоящее время включена.</span><span class="sxs-lookup"><span data-stu-id="2fb92-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="2fb92-197">`$even` и `$odd`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-197">`$even` and `$odd`</span></span>

<span data-ttu-id="2fb92-198">Используйте `$even` в `ng-repeat` цикла, чтобы определить, является ли текущий индекс в цикле даже индексированных строк.</span><span class="sxs-lookup"><span data-stu-id="2fb92-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="2fb92-199">Аналогичным образом, использовать `$odd` чтобы определить, является ли текущий индекс нечетных индексированных строк.</span><span class="sxs-lookup"><span data-stu-id="2fb92-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="2fb92-200">`$first` и `$last`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-200">`$first` and `$last`</span></span>

<span data-ttu-id="2fb92-201">Используйте `$first` в `ng-repeat` цикла, чтобы определить, является ли текущий индекс в цикле первой строки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="2fb92-202">Аналогичным образом, использовать `$last` , чтобы определить, является ли текущий индекс последней строки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="2fb92-203">Ниже приведен пример, показывающий `$index`, `$even`, `$odd`, `$first`, и `$last` в действии:</span><span class="sxs-lookup"><span data-stu-id="2fb92-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="2fb92-204">Ниже приведен результат:</span><span class="sxs-lookup"><span data-stu-id="2fb92-204">Here is the resulting output:</span></span>

![Пример повторителя 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="2fb92-206">$scope</span><span class="sxs-lookup"><span data-stu-id="2fb92-206">$scope</span></span>

<span data-ttu-id="2fb92-207">`$scope`представляет собой объект JavaScript, который выступает в качестве связующего между представлением (шаблон) и контроллер (описано ниже).</span><span class="sxs-lookup"><span data-stu-id="2fb92-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="2fb92-208">Шаблон представления в AngularJS только известны значения, присоединенных к `$scope` объекта в контроллере.</span><span class="sxs-lookup"><span data-stu-id="2fb92-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="2fb92-209">В мире MVVM `$scope` объект в AngularJS часто определяется как ViewModel.</span><span class="sxs-lookup"><span data-stu-id="2fb92-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="2fb92-210">Команда AngularJS ссылается на `$scope` объект в качестве модели данных.</span><span class="sxs-lookup"><span data-stu-id="2fb92-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="2fb92-211">[Дополнительные сведения об областях в AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="2fb92-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="2fb92-212">Ниже приведен простой пример, показывающий, как задать свойства для `$scope` в отдельном файле JavaScript, *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="2fb92-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="2fb92-213">Наблюдать за `$scope` передать параметр в строке 2 контроллера.</span><span class="sxs-lookup"><span data-stu-id="2fb92-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="2fb92-214">Этот объект является знает о представлении.</span><span class="sxs-lookup"><span data-stu-id="2fb92-214">This object is what the view knows about.</span></span> <span data-ttu-id="2fb92-215">В строке 3 рекомендуется задать свойство с именем «name» в «Mary Jane».</span><span class="sxs-lookup"><span data-stu-id="2fb92-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="2fb92-216">Что происходит, когда определенное свойство, не найден в представлении?</span><span class="sxs-lookup"><span data-stu-id="2fb92-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="2fb92-217">Свойства «имя» и «возраст» ссылается представление, определенное ниже:</span><span class="sxs-lookup"><span data-stu-id="2fb92-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="2fb92-218">Обратите внимание, в строке 9, что мы просим угловая отображается свойство «имя», с использованием синтаксиса выражений.</span><span class="sxs-lookup"><span data-stu-id="2fb92-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="2fb92-219">Строка 10 затем ссылается на «age», свойство, которое не существует.</span><span class="sxs-lookup"><span data-stu-id="2fb92-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="2fb92-220">Выполнение примере именем, равным «Мэри Jane» и ничего для возраста.</span><span class="sxs-lookup"><span data-stu-id="2fb92-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="2fb92-221">Отсутствующие свойства игнорируются.</span><span class="sxs-lookup"><span data-stu-id="2fb92-221">Missing properties are ignored.</span></span>

![Пример области](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="2fb92-223">Модули</span><span class="sxs-lookup"><span data-stu-id="2fb92-223">Modules</span></span>

<span data-ttu-id="2fb92-224">Объект [модуль](https://docs.angularjs.org/guide/module) в AngularJS — это совокупность контроллеров, службы, директивы, и т. д. `angular.module()` Вызов функции используется для создания, регистрации и получения модулей AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="2fb92-225">Все модули, включая те, поставляемый корпорацией AngularJS команды и библиотек сторонних производителей, должен быть зарегистрирован с помощью `angular.module()` функции.</span><span class="sxs-lookup"><span data-stu-id="2fb92-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="2fb92-226">Ниже приведен фрагмент кода, показывающий, как создать новый модуль в AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="2fb92-227">Первый параметр является имя модуля.</span><span class="sxs-lookup"><span data-stu-id="2fb92-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="2fb92-228">Второй параметр определяет зависимости на другие модули.</span><span class="sxs-lookup"><span data-stu-id="2fb92-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="2fb92-229">Далее в этой статье мы будет отображаться как передавать эти зависимости `angular.module()` вызова метода.</span><span class="sxs-lookup"><span data-stu-id="2fb92-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="2fb92-230">Используйте `ng-app` директивы для представления модуль AngularJS на странице.</span><span class="sxs-lookup"><span data-stu-id="2fb92-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="2fb92-231">Чтобы использовать модуль, назначить имя модуля, `personApp` в этом примере для `ng-app` директив в нашем шаблона.</span><span class="sxs-lookup"><span data-stu-id="2fb92-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="2fb92-232">Контроллеры</span><span class="sxs-lookup"><span data-stu-id="2fb92-232">Controllers</span></span>

<span data-ttu-id="2fb92-233">[Контроллеры](https://docs.angularjs.org/guide/controller) в AngularJS, первой точкой входа для кода.</span><span class="sxs-lookup"><span data-stu-id="2fb92-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="2fb92-234">`<module name>.controller()` Вызов функции используется для создания и регистрации контроллеров в AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="2fb92-235">`ng-controller` Директива используется для представления контроллер AngularJS на HTML-странице.</span><span class="sxs-lookup"><span data-stu-id="2fb92-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="2fb92-236">Роли контроллера в угловой является установка состояние и поведение модели данных (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="2fb92-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="2fb92-237">Контроллеры не должен использоваться для управления DOM напрямую.</span><span class="sxs-lookup"><span data-stu-id="2fb92-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="2fb92-238">Ниже приведен фрагмент кода, который регистрирует новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="2fb92-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="2fb92-239">`personApp` Переменная в фрагменте ссылается углового модуль, который определен в строке 2.</span><span class="sxs-lookup"><span data-stu-id="2fb92-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="2fb92-240">Представление с помощью `ng-controller` директива назначает имя контроллера:</span><span class="sxs-lookup"><span data-stu-id="2fb92-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="2fb92-241">На странице отображается «Мария» и «Jane», которые соответствуют `firstName` и `lastName` свойства присоединяется к `$scope` объекта:</span><span class="sxs-lookup"><span data-stu-id="2fb92-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Пример контроллера](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="2fb92-243">Компоненты</span><span class="sxs-lookup"><span data-stu-id="2fb92-243">Components</span></span>

<span data-ttu-id="2fb92-244">[Компоненты](https://docs.angularjs.org/guide/component) в угловой 1.5.x разрешить для инкапсуляции и возможность создания отдельного HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="2fb92-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="2fb92-245">В угловой 1.4.x удалось добиться одной функции, с помощью метода .directive().</span><span class="sxs-lookup"><span data-stu-id="2fb92-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="2fb92-246">С помощью метода .component(), разработки упрощено получение функциональные возможности директивы и контроллера.</span><span class="sxs-lookup"><span data-stu-id="2fb92-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="2fb92-247">Другие преимущества —; изоляция на уровне области, принадлежат рекомендации и миграции углового 2 становится проще задачу.</span><span class="sxs-lookup"><span data-stu-id="2fb92-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="2fb92-248">`<module name>.component()` Вызов функции используется для создания и регистрации компонентов в AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="2fb92-249">Ниже приведен фрагмент кода, который регистрирует новый компонент.</span><span class="sxs-lookup"><span data-stu-id="2fb92-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="2fb92-250">`personApp` Переменная в фрагменте ссылается углового модуль, который определен в строке 2.</span><span class="sxs-lookup"><span data-stu-id="2fb92-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="2fb92-251">Представление, где отображается пользовательский элемент HTML.</span><span class="sxs-lookup"><span data-stu-id="2fb92-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="2fb92-252">Связанный шаблон, который используется компонентом:</span><span class="sxs-lookup"><span data-stu-id="2fb92-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="2fb92-253">На странице отображается «Aftab» и «Ansari», которые соответствуют `firstName` и `lastName` свойства присоединяется к `vm` объекта:</span><span class="sxs-lookup"><span data-stu-id="2fb92-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Пример компонентов](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="2fb92-255">Службы</span><span class="sxs-lookup"><span data-stu-id="2fb92-255">Services</span></span>

<span data-ttu-id="2fb92-256">[Службы](https://docs.angularjs.org/guide/services) в AngularJS обычно используются для общего кода, который абстрагироваться в файл, который может использоваться в течение времени существования углового приложения.</span><span class="sxs-lookup"><span data-stu-id="2fb92-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="2fb92-257">Неактивно создаются службы, это означает, что не будет экземпляр службы, пока не будет использоваться компонентом, который зависит от службы.</span><span class="sxs-lookup"><span data-stu-id="2fb92-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="2fb92-258">Фабрики являются примером службы, используемые в приложениях AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="2fb92-259">Фабрики создаются с помощью `myApp.factory()` функции вызова, где `myApp` — модуль.</span><span class="sxs-lookup"><span data-stu-id="2fb92-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="2fb92-260">Ниже приведен пример, демонстрирующий использование фабрик в AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="2fb92-261">Для вызова этой фабрики от контроллера, передачи `personFactory` как параметр `controller` функции:</span><span class="sxs-lookup"><span data-stu-id="2fb92-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="2fb92-262">Можно также обратиться к конечной точке REST с помощью служб</span><span class="sxs-lookup"><span data-stu-id="2fb92-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="2fb92-263">Ниже — законченный пример использует службы в AngularJS для взаимодействия с конечной точкой веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2fb92-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="2fb92-264">В примере получает данные из веб-API и данные отображаются в Просмотр шаблона.</span><span class="sxs-lookup"><span data-stu-id="2fb92-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="2fb92-265">Начнем с создания представления сначала:</span><span class="sxs-lookup"><span data-stu-id="2fb92-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="2fb92-266">В этом представлении у нас есть углового модуль, называемый `PersonsApp` и вызывается контроллер `personController`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="2fb92-267">Мы используем `ng-repeat` для прохода по списку лиц.</span><span class="sxs-lookup"><span data-stu-id="2fb92-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="2fb92-268">Мы ссылаются три пользовательских файлов JavaScript в строках 17-19.</span><span class="sxs-lookup"><span data-stu-id="2fb92-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="2fb92-269">*PersonApp.js* файл используется для регистрации `PersonsApp` модуль; и синтаксис аналогично предыдущих примерах.</span><span class="sxs-lookup"><span data-stu-id="2fb92-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="2fb92-270">Мы используем `angular.module` функцию, чтобы создать новый экземпляр модуля, мы будем работать с.</span><span class="sxs-lookup"><span data-stu-id="2fb92-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="2fb92-271">Давайте рассмотрим *personFactory.js*ниже.</span><span class="sxs-lookup"><span data-stu-id="2fb92-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="2fb92-272">Поступает вызов модуля `factory` метод для создания фабрики.</span><span class="sxs-lookup"><span data-stu-id="2fb92-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="2fb92-273">Строка 12 показывает встроенных угловая `$http` службы, получение сведений о людей из веб-службы.</span><span class="sxs-lookup"><span data-stu-id="2fb92-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="2fb92-274">В *personController.js*, поступает вызов модуля `controller` метод для создания контроллера.</span><span class="sxs-lookup"><span data-stu-id="2fb92-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="2fb92-275">`$scope` Объекта `people` присваивается данные, возвращенные из personFactory (строка 13).</span><span class="sxs-lookup"><span data-stu-id="2fb92-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="2fb92-276">Давайте кратко рассмотрим веб-API и модель, стоящий за ней.</span><span class="sxs-lookup"><span data-stu-id="2fb92-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="2fb92-277">`Person` Модель является POCO (Plain объект среды CLR) с `Id`, `FirstName`, и `LastName` свойства:</span><span class="sxs-lookup"><span data-stu-id="2fb92-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="2fb92-278">`Person` Контроллера возвращает список формата JSON `Person` объектов:</span><span class="sxs-lookup"><span data-stu-id="2fb92-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="2fb92-279">Давайте посмотрим, приложение в действии:</span><span class="sxs-lookup"><span data-stu-id="2fb92-279">Let's see the application in action:</span></span>

![Отображение результатов REST контроллера](angular/_static/rest-bound.png)

<span data-ttu-id="2fb92-281">Вы можете [просмотреть структуру приложения на GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="2fb92-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="2fb92-282">Дополнительные сведения о структурировании приложения AngularJS см [Джон Папа углового по стилю](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="2fb92-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="2fb92-283">Чтобы создать модуль AngularJS, контроллер, фабрики, директивы и просмотр файлов легко, нужно убедиться, что извлечь Sayed Hashimi [SideWaffle шаблон пакета для Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="2fb92-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="2fb92-284">Sayed Hashimi — старший руководитель программы в Visual Studio Team Web корпорации Майкрософт и шаблоны SideWaffle считаются gold standard.</span><span class="sxs-lookup"><span data-stu-id="2fb92-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="2fb92-285">На момент написания этой статьи SideWaffle доступна для Visual Studio 2012, 2013 и 2015.</span><span class="sxs-lookup"><span data-stu-id="2fb92-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="2fb92-286">Служба маршрутизации и несколько представлений</span><span class="sxs-lookup"><span data-stu-id="2fb92-286">Routing and multiple views</span></span>

<span data-ttu-id="2fb92-287">AngularJS имеет встроенные маршрут поставщика для обработки SPA (одностраничного приложения) на основе переходов.</span><span class="sxs-lookup"><span data-stu-id="2fb92-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="2fb92-288">Для работы с маршрутизацией в AngularJS, необходимо добавить `angular-route` библиотеки с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="2fb92-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="2fb92-289">Вы увидите на [bower.json](#angular-bower-json) файл, указанный в начале этой статьи, что мы уже на нее имеются ссылки в проект.</span><span class="sxs-lookup"><span data-stu-id="2fb92-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="2fb92-290">После установки пакета добавьте ссылку на скрипт (*angular route.js*) для представления.</span><span class="sxs-lookup"><span data-stu-id="2fb92-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="2fb92-291">Теперь давайте лица приложения мы было создание и добавление навигации.</span><span class="sxs-lookup"><span data-stu-id="2fb92-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="2fb92-292">Во-первых, будет выполнено копирование приложения путем создания нового `PeopleController` действие, вызванное `Spa` и соответствующим `Spa.cshtml` представления путем копирования в представлении Index.cshtml в `People` папки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="2fb92-293">Добавьте ссылку на скрипт `angular-route` (см. строку 11).</span><span class="sxs-lookup"><span data-stu-id="2fb92-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="2fb92-294">Также добавьте `div` , отмеченные `ng-view` директивы (см. строку 6) как заполнитель для размещения представлений в.</span><span class="sxs-lookup"><span data-stu-id="2fb92-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="2fb92-295">Мы будем использовать некоторые дополнительные *.js* файлы, на которые ссылаются на строках 13-16.</span><span class="sxs-lookup"><span data-stu-id="2fb92-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="2fb92-296">Давайте рассмотрим *personModule.js* файл, чтобы увидеть, как мы Создание модуля с маршрутизацией.</span><span class="sxs-lookup"><span data-stu-id="2fb92-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="2fb92-297">Мы передаем `ngRoute` как библиотеку в модуль.</span><span class="sxs-lookup"><span data-stu-id="2fb92-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="2fb92-298">Этот модуль обработку маршрутизации в приложении.</span><span class="sxs-lookup"><span data-stu-id="2fb92-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="2fb92-299">*PersonRoutes.js* файла, ниже, определяет маршруты, в зависимости от поставщика маршрута.</span><span class="sxs-lookup"><span data-stu-id="2fb92-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="2fb92-300">Строки 4-7 определение навигации произнеся эффективно, если URL-адрес с `/persons` — запрошено, используйте шаблон называется `partials/personlist` при одновременной ликвидации `personListController`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="2fb92-301">Строки с 8 по 11 указывают страницу сведений с параметром маршрута `personId`.</span><span class="sxs-lookup"><span data-stu-id="2fb92-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="2fb92-302">Если URL-адрес не соответствует одному из шаблонов, угловая по умолчанию `/persons` представления.</span><span class="sxs-lookup"><span data-stu-id="2fb92-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="2fb92-303">`personlist.html` Файл является частичное представление, содержащее только HTML, чтобы отобразить список людей.</span><span class="sxs-lookup"><span data-stu-id="2fb92-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="2fb92-304">Контроллер определяется с помощью модуля `controller` функционировать в *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="2fb92-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="2fb92-305">Если запустить это приложение и перейдите к `people/spa#/persons` URL-адрес, мы увидим:</span><span class="sxs-lookup"><span data-stu-id="2fb92-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Представление списка лиц](angular/_static/spa-persons.png)

<span data-ttu-id="2fb92-307">При переходе на страницу сведений, например `people/spa#/persons/2`, мы увидим частичного представления сведений:</span><span class="sxs-lookup"><span data-stu-id="2fb92-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Подробное представление Person](angular/_static/spa-persons-2.png)

<span data-ttu-id="2fb92-309">Можно просмотреть полный исходный и все файлы, не отображаются в этой статье на [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="2fb92-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="2fb92-310">Обработчики событий</span><span class="sxs-lookup"><span data-stu-id="2fb92-310">Event Handlers</span></span>

<span data-ttu-id="2fb92-311">Существует несколько директив в AngularJS, добавить возможности обработки событий в входных элементов в вашей модели HTML DOM.</span><span class="sxs-lookup"><span data-stu-id="2fb92-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="2fb92-312">Ниже приведен список событий, которые встроены в AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-312">Below is a list of the events that are built into AngularJS.</span></span>

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> <span data-ttu-id="2fb92-313">Можно добавить собственные обработчики событий с помощью [пользовательских директив компонентов в AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="2fb92-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="2fb92-314">Давайте взглянем на как `ng-click` построенная событий.</span><span class="sxs-lookup"><span data-stu-id="2fb92-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="2fb92-315">Создайте новый файл JavaScript с именем *eventHandlerController.js*и добавьте в него следующее:</span><span class="sxs-lookup"><span data-stu-id="2fb92-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="2fb92-316">Обратите внимание на новое `sayName` функционировать в `eventHandlerController` в строке 5 выше.</span><span class="sxs-lookup"><span data-stu-id="2fb92-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="2fb92-317">Выполняет метод для теперь отображается предупреждение JavaScript для пользователя без приветственное сообщение.</span><span class="sxs-lookup"><span data-stu-id="2fb92-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="2fb92-318">Представления привязывает функции контроллера AngularJS события.</span><span class="sxs-lookup"><span data-stu-id="2fb92-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="2fb92-319">Строки 9 есть кнопка, на котором `ng-click` применен угловой директивы.</span><span class="sxs-lookup"><span data-stu-id="2fb92-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="2fb92-320">Он вызывает нашей `sayName` функции, которая присоединяется к `$scope` объект, переданный в этом представлении.</span><span class="sxs-lookup"><span data-stu-id="2fb92-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="2fb92-321">Пример выполнения показывает, что контроллер `sayName` функция вызывается автоматически при нажатии кнопки.</span><span class="sxs-lookup"><span data-stu-id="2fb92-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Click-событие](angular/_static/events.png)

<span data-ttu-id="2fb92-323">Дополнительные сведения о AngularJS Встроенные события обработчика директив нужно убедиться, что заголовок, чтобы [веб-сайт документации](https://docs.angularjs.org/api/ng/directive/ngClick) из AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2fb92-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2fb92-324">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2fb92-324">Additional resources</span></span>

* [<span data-ttu-id="2fb92-325">Угловое документы</span><span class="sxs-lookup"><span data-stu-id="2fb92-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="2fb92-326">Угловое Info 2</span><span class="sxs-lookup"><span data-stu-id="2fb92-326">Angular 2 Info</span></span>](https://angular.io/)
