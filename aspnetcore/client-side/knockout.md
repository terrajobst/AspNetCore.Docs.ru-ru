---
title: "Knockout.js MVVM Framework в ASP.NET Core"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="b524d-103">Knockout.js MVVM Framework в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b524d-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="b524d-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b524d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b524d-105">Маскирования — это популярных библиотека JavaScript, которая упрощает создание сложных данных пользовательских интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="b524d-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="b524d-106">Он может использоваться отдельно или совместно с другими библиотеками, например jQuery.</span><span class="sxs-lookup"><span data-stu-id="b524d-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="b524d-107">Ее основная задача — для привязки элементов пользовательского интерфейса базовую модель данных определяется как объект JavaScript, таким образом, что при внесении изменений в пользовательском Интерфейсе, модель обновляется и наоборот.</span><span class="sxs-lookup"><span data-stu-id="b524d-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="b524d-108">Маскировать облегчает использование шаблона Model-View-ViewModel (MVVM) в поведении клиентские веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="b524d-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="b524d-109">Две основные концепции, которые один нужно знать при работе с реализацией MVVM маскирования являются наблюдаемые объекты и привязки.</span><span class="sxs-lookup"><span data-stu-id="b524d-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b524d-110">Начало работы</span><span class="sxs-lookup"><span data-stu-id="b524d-110">Getting started</span></span>

<span data-ttu-id="b524d-111">Маскировать развертывается как отдельный файл JavaScript, поэтому установка, и его использования является очень простым с использованием [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="b524d-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="b524d-112">При условии, что у вас уже есть [bower](bower.md) и [gulp](using-gulp.md) , откройте *bower.json* в ваш ASP.NET Core проектов и добавить зависимость маскирования, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="b524d-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

<span data-ttu-id="b524d-113">Это место после этого можно вручную выполнить bower, открыв диспетчер выполнения задач (в представлении ‣ ‣ другие окна Task Runner Explorer) в области задач, щелкните правой кнопкой мыши на bower и выберите выполнения.</span><span class="sxs-lookup"><span data-stu-id="b524d-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="b524d-114">Результат должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b524d-114">The result should appear similar to this:</span></span>

![bower работает маскирования в Task Runner Explorer](knockout/_static/bower-knockout.png)

<span data-ttu-id="b524d-116">Теперь, если искать в своем проекте `wwwroot` папку, вы увидите маскирования установлены в эту папку.</span><span class="sxs-lookup"><span data-stu-id="b524d-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![устанавливается в папку lib маскирования](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="b524d-118">Рекомендуется в рабочей среде ссылаться маскирования через сеть доставки содержимого или CDN, как это повышает вероятность того, что пользователи уже будет кэшированную копию файла и таким образом не потребуется загрузить его вообще.</span><span class="sxs-lookup"><span data-stu-id="b524d-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="b524d-119">Маскировать доступна на нескольких CDN, включая Microsoft Ajax CDN, здесь:</span><span class="sxs-lookup"><span data-stu-id="b524d-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="b524d-120">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="b524d-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="b524d-121">Чтобы включить маскирования на странице, которая будет его использовать, просто добавьте `<script>` ссылающийся на файл из везде, где можно будет размещен ее (вместе с приложением или через CDN):</span><span class="sxs-lookup"><span data-stu-id="b524d-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="b524d-122">Наблюдаемые объекты, ViewModels и простая привязка</span><span class="sxs-lookup"><span data-stu-id="b524d-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="b524d-123">Вы уже знакомы с использованием JavaScript для управления элементами на веб-странице, либо с помощью прямой доступ к модели DOM или библиотеки, например jQuery.</span><span class="sxs-lookup"><span data-stu-id="b524d-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="b524d-124">Обычно такое поведение достигается путем написания кода для прямого присвоения значений элементов в ответ на определенные действия пользователя.</span><span class="sxs-lookup"><span data-stu-id="b524d-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="b524d-125">С помощью Knockout декларативный подход берется вместо, через который элементов на странице, привязанные к свойствам объекта.</span><span class="sxs-lookup"><span data-stu-id="b524d-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="b524d-126">Вместо написания кода для управления элементами DOM, действия пользователя для взаимодействия с объектом ViewModel просто и маскирования берет на себя убедитесь, что синхронизируются элементы страницы.</span><span class="sxs-lookup"><span data-stu-id="b524d-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="b524d-127">В качестве простого примера рассмотрим страницу списка.</span><span class="sxs-lookup"><span data-stu-id="b524d-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="b524d-128">Он включает `<span>` элемент с `data-bind` атрибут, указывающий, что текстовое содержимое должно быть привязано к authorName.</span><span class="sxs-lookup"><span data-stu-id="b524d-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="b524d-129">Далее, в блоке JavaScript с единственным свойством определяется переменной viewModel `authorName`, принимает значение.</span><span class="sxs-lookup"><span data-stu-id="b524d-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="b524d-130">Наконец, вызов `ko.applyBindings` производится, передав в этой переменной viewModel.</span><span class="sxs-lookup"><span data-stu-id="b524d-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

<span data-ttu-id="b524d-131">При просмотре в браузере, а содержимое <span> элемент заменяется значением переменной viewModel:</span><span class="sxs-lookup"><span data-stu-id="b524d-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![Простая привязка маскирования](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="b524d-133">Теперь у нас есть рабочий простой односторонние привязки.</span><span class="sxs-lookup"><span data-stu-id="b524d-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="b524d-134">Обратите внимание, что нигде в коде мы писали JavaScript для присвоения значения в области содержимого.</span><span class="sxs-lookup"><span data-stu-id="b524d-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="b524d-135">Если требуется управлять ViewModel, мы можно сделать это шаг дальнейшей и добавьте текстовое поле ввода HTML и, как привязать значение по таким:</span><span class="sxs-lookup"><span data-stu-id="b524d-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="b524d-136">Перезагрузить страницу, мы увидим, что это значение действительно привязывается к поле ввода:</span><span class="sxs-lookup"><span data-stu-id="b524d-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![Привязка ввода маскирования](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="b524d-138">Однако если изменить значение в текстовом поле, соответствующее значение в `<span>` элемента не изменяется.</span><span class="sxs-lookup"><span data-stu-id="b524d-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="b524d-139">Почему?</span><span class="sxs-lookup"><span data-stu-id="b524d-139">Why not?</span></span>

<span data-ttu-id="b524d-140">Проблема в том, ничего не уведомляется `<span>` , он не требуется обновлять.</span><span class="sxs-lookup"><span data-stu-id="b524d-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="b524d-141">Достаточно обновить ViewModel не сам по себе недостаточно, если свойства ViewModel упаковываются в специальный тип.</span><span class="sxs-lookup"><span data-stu-id="b524d-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="b524d-142">Необходимо использовать **наблюдаемые объекты** в ViewModel для всех свойств, которые должны иметь изменения автоматически обновляется по мере их появления.</span><span class="sxs-lookup"><span data-stu-id="b524d-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="b524d-143">Изменив ViewModel использование `ko.observable("value")` вместо просто «value», ViewModel обновит HTML-элементов, привязанных к значению, при каждом изменении.</span><span class="sxs-lookup"><span data-stu-id="b524d-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="b524d-144">Обратите внимание, что полей ввода не обновлять их значения до потери фокуса, поэтому вы не увидите изменения в привязанных элементов при вводе.</span><span class="sxs-lookup"><span data-stu-id="b524d-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="b524d-145">Добавление поддержки динамическое обновление, после каждого нажатия клавиши — это просто добавить `valueUpdate: "afterkeydown"` для `data-bind` содержимое атрибута.</span><span class="sxs-lookup"><span data-stu-id="b524d-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="b524d-146">Это поведение можно также получить с помощью `data-bind="textInput: authorName"` мгновенного обновления значений.</span><span class="sxs-lookup"><span data-stu-id="b524d-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="b524d-147">Наш viewModel после ее для использования ko.observable обновления:</span><span class="sxs-lookup"><span data-stu-id="b524d-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="b524d-148">Маскировать поддерживает несколько различных видов привязок.</span><span class="sxs-lookup"><span data-stu-id="b524d-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="b524d-149">Пока мы рассмотрели для привязки к `text` и `value`.</span><span class="sxs-lookup"><span data-stu-id="b524d-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="b524d-150">Также можно привязать к любой данного атрибута.</span><span class="sxs-lookup"><span data-stu-id="b524d-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="b524d-151">Например, чтобы создать гиперссылку с тег, `src` атрибута может быть привязано к viewModel.</span><span class="sxs-lookup"><span data-stu-id="b524d-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="b524d-152">Маскировать также поддерживает привязку к функции.</span><span class="sxs-lookup"><span data-stu-id="b524d-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="b524d-153">Чтобы продемонстрировать это, давайте обновления viewModel для включения маркера twitter автора и отображать как ссылку на страницу twitter автора маркер twitter.</span><span class="sxs-lookup"><span data-stu-id="b524d-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="b524d-154">Нам предстоит выполнить в три этапа.</span><span class="sxs-lookup"><span data-stu-id="b524d-154">We'll do this in three stages.</span></span>

<span data-ttu-id="b524d-155">Сначала добавьте HTML для отображения гиперссылки, которой будет показано в скобках после имени автора:</span><span class="sxs-lookup"><span data-stu-id="b524d-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="b524d-156">Затем обновите viewModel, чтобы включить свойства twitterUrl и twitterAlias:</span><span class="sxs-lookup"><span data-stu-id="b524d-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="b524d-157">Обратите внимание, что на этом этапе мы еще не обновлены twitterUrl, чтобы перейти на правильный URL-адрес для данного псевдонима twitter — просто обращены twitter.com. Также Обратите внимание, что мы используем новая функция маскирования `computed`, для twitterUrl.</span><span class="sxs-lookup"><span data-stu-id="b524d-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com. Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="b524d-158">Это — это наблюдаемый функция, которая будет уведомлять все элементы пользовательского интерфейса, если он изменяет.</span><span class="sxs-lookup"><span data-stu-id="b524d-158">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="b524d-159">Тем не менее оно должно иметь доступ к другим свойствам в viewModel, нам нужно изменить, как мы создаем viewModel, таким образом, чтобы каждое свойство собственную инструкцию.</span><span class="sxs-lookup"><span data-stu-id="b524d-159">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="b524d-160">Ниже показан измененный viewModel объявления.</span><span class="sxs-lookup"><span data-stu-id="b524d-160">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="b524d-161">Теперь он объявляется как функция.</span><span class="sxs-lookup"><span data-stu-id="b524d-161">It is now declared as a function.</span></span> <span data-ttu-id="b524d-162">Обратите внимание, что каждое свойство собственную инструкцию теперь заканчивается точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="b524d-162">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="b524d-163">Кроме того, для доступа к значению свойства twitterAlias, необходимо выполнить его, поэтому его ссылка включает в себя ().</span><span class="sxs-lookup"><span data-stu-id="b524d-163">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="b524d-164">Результат работы должным образом в браузере.</span><span class="sxs-lookup"><span data-stu-id="b524d-164">The result works as expected in the browser:</span></span>

![Гиперссылка маскирования](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="b524d-166">Маскировать также поддерживает привязку к определенные события элемент пользовательского интерфейса, такие как события щелчка.</span><span class="sxs-lookup"><span data-stu-id="b524d-166">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="b524d-167">Это позволяет легко и декларативно привязывать элементы пользовательского интерфейса для функций внутри приложения viewModel.</span><span class="sxs-lookup"><span data-stu-id="b524d-167">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="b524d-168">В качестве простого примера можно добавить кнопку, при нажатии, изменяет автора twitterAlias быть прописными буквами.</span><span class="sxs-lookup"><span data-stu-id="b524d-168">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="b524d-169">Во-первых, мы добавьте кнопки, привязки на кнопку событий, а также ссылки на имя функции, которую мы собираемся добавить viewModel:</span><span class="sxs-lookup"><span data-stu-id="b524d-169">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="b524d-170">Затем добавьте в функцию viewModel и подключить его для изменения состояния viewModel.</span><span class="sxs-lookup"><span data-stu-id="b524d-170">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="b524d-171">Обратите внимание, что присваивается новое значение свойства twitterAlias, мы вызвать в качестве метода передайте новое значение.</span><span class="sxs-lookup"><span data-stu-id="b524d-171">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="b524d-172">Выполнение кода и нажав кнопку изменяет отображаемая ссылка должным образом:</span><span class="sxs-lookup"><span data-stu-id="b524d-172">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![преобразовать в прописную гиперссылки](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="b524d-174">Поток управления</span><span class="sxs-lookup"><span data-stu-id="b524d-174">Control flow</span></span>

<span data-ttu-id="b524d-175">Маскирования содержит привязок, которые могут выполнять операции условного и циклы.</span><span class="sxs-lookup"><span data-stu-id="b524d-175">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="b524d-176">Циклические операции особенно полезны для привязки списков данных для пользовательского интерфейса списки, меню и таблицы или таблиц.</span><span class="sxs-lookup"><span data-stu-id="b524d-176">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="b524d-177">Привязка по каждому элементу будет применен к массиву.</span><span class="sxs-lookup"><span data-stu-id="b524d-177">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="b524d-178">При использовании с массивом наблюдаемый автоматически обновит элементы пользовательского интерфейса, когда элементы добавлены или удалены из массива, без повторного создания каждого элемента в дереве пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b524d-178">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="b524d-179">В следующем примере используется новый viewModel, включая наблюдаемый массив результатов игры.</span><span class="sxs-lookup"><span data-stu-id="b524d-179">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="b524d-180">Он связан с простой таблицы с двумя столбцами с помощью `foreach` привязки для `<tbody>` элемента.</span><span class="sxs-lookup"><span data-stu-id="b524d-180">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="b524d-181">Каждый `<tr>` элемент в пределах `<tbody>` будет привязано к элементу коллекции gameResults.</span><span class="sxs-lookup"><span data-stu-id="b524d-181">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

<span data-ttu-id="b524d-182">Обратите внимание, что это время мы используем ViewModel с заглавной буквы «V» так, как ожидается, что для создания с помощью «new» (в вызове applyBindings).</span><span class="sxs-lookup"><span data-stu-id="b524d-182">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="b524d-183">При выполнении произведут следующий результат:</span><span class="sxs-lookup"><span data-stu-id="b524d-183">When executed, the page results in the following output:</span></span>

![модель представления записей маскирования](knockout/_static/record-screenshot.png)

<span data-ttu-id="b524d-185">Для демонстрации функциональности наблюдаемой коллекции, добавим немного более широкими функциональными возможностями.</span><span class="sxs-lookup"><span data-stu-id="b524d-185">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="b524d-186">Мы позволяют записывать результаты другого игры ViewModel и затем добавить кнопки и пользовательский Интерфейс для работы с этой новой функции.</span><span class="sxs-lookup"><span data-stu-id="b524d-186">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="b524d-187">Во-первых давайте создадим метод addResult:</span><span class="sxs-lookup"><span data-stu-id="b524d-187">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="b524d-188">Этот метод Bind кнопки с помощью `click` привязки:</span><span class="sxs-lookup"><span data-stu-id="b524d-188">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="b524d-189">Откройте страницу в браузере и нажмите кнопку несколько раз, что приводит к новой строки каждого щелчка:</span><span class="sxs-lookup"><span data-stu-id="b524d-189">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Добавьте к результатам](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="b524d-191">Существует несколько способов для поддержки добавления новых записей в пользовательском Интерфейсе, обычно либо встроенной или в отдельную форму.</span><span class="sxs-lookup"><span data-stu-id="b524d-191">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="b524d-192">Можно легко изменять таблицу в текстовые поля и элементами управления DropDownList таким образом, чтобы его для редактирования.</span><span class="sxs-lookup"><span data-stu-id="b524d-192">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="b524d-193">Замените `<tr>` элемента, как показано:</span><span class="sxs-lookup"><span data-stu-id="b524d-193">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="b524d-194">Обратите внимание, что `$root` означает корневой каталог ViewModel, который является, где предоставляются возможные варианты.</span><span class="sxs-lookup"><span data-stu-id="b524d-194">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="b524d-195">`$data`ссылается на независимо от текущей модели — в данном контексте — в данном случае он относится к отдельному элементу массива resultChoices, каждый из которых представляет собой простую строку.</span><span class="sxs-lookup"><span data-stu-id="b524d-195">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="b524d-196">Благодаря этому изменению всей сетки становится доступным для редактирования:</span><span class="sxs-lookup"><span data-stu-id="b524d-196">With this change, the entire grid becomes editable:</span></span>

![Редактируемая сетка](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="b524d-198">Если мы не использовали маскирования, мы позволяет достигнуть все это с помощью jQuery, но скорее всего не может быть практически столь же эффективна.</span><span class="sxs-lookup"><span data-stu-id="b524d-198">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="b524d-199">Маскировать отслеживает какие связанные данные, соответствующие элементы в ViewModel какие элементы пользовательского интерфейса и обновляет только те элементы, которые нужно добавить, удалить или обновить.</span><span class="sxs-lookup"><span data-stu-id="b524d-199">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="b524d-200">Может потребоваться значительные усилия, чтобы добиться этого, сами с помощью jQuery или прямого управления DOM и даже если затем мы хотели отображения статистических результатов (например, потери запись) на основе данных таблицы, необходимо еще раз перебор его и выполнять синтаксический анализ HTML-элементов.</span><span class="sxs-lookup"><span data-stu-id="b524d-200">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="b524d-201">С помощью Knockout отображения-потери записи является тривиальным.</span><span class="sxs-lookup"><span data-stu-id="b524d-201">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="b524d-202">Мы вычислений в пределах самого ViewModel и его отображения с привязкой простого текста и `<span>`.</span><span class="sxs-lookup"><span data-stu-id="b524d-202">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="b524d-203">Для создания строки потери записи, мы используем вычисляемый наблюдаемым.</span><span class="sxs-lookup"><span data-stu-id="b524d-203">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="b524d-204">Обратите внимание, что ссылки на наблюдаемый свойства ViewModel должны быть вызовы функций, в противном случае они не получит значение наблюдаемым (т. е. `gameResults()` не `gameResults` в коде, показанном):</span><span class="sxs-lookup"><span data-stu-id="b524d-204">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="b524d-205">Привязки к диапазону в пределах этой функции `<h1>` элемент в верхней части страницы:</span><span class="sxs-lookup"><span data-stu-id="b524d-205">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="b524d-206">Результат:</span><span class="sxs-lookup"><span data-stu-id="b524d-206">The result:</span></span>

![Потери](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="b524d-208">Добавление строк или изменения выбранного элемента в любой строке столбца результирующего будет обновить запись в верхней части окна.</span><span class="sxs-lookup"><span data-stu-id="b524d-208">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="b524d-209">Помимо привязки к значениям также можно использовать практически любой юридических выражение JavaScript, в привязке.</span><span class="sxs-lookup"><span data-stu-id="b524d-209">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="b524d-210">Например если элемент пользовательского интерфейса отображается только при определенных условиях, например, если значение превышает определенное пороговое значение, можно указать это логически в выражение привязки:</span><span class="sxs-lookup"><span data-stu-id="b524d-210">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="b524d-211">Это `<div>` будет показана только при customerValue — более 100.</span><span class="sxs-lookup"><span data-stu-id="b524d-211">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="b524d-212">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="b524d-212">Templates</span></span>

<span data-ttu-id="b524d-213">Маскировать включает поддержку для шаблонов, таким образом, можно легко разделить пользовательского интерфейса из поведение или добавочной загрузки элементов пользовательского интерфейса в большого приложения по требованию.</span><span class="sxs-lookup"><span data-stu-id="b524d-213">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="b524d-214">Корпорация Майкрософт может обновлять наш пример для создания своего собственного шаблона каждой строки, просто помещает удаление HTML в шаблон и указание шаблона по имени в вызове привязка к данным на `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="b524d-214">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

<span data-ttu-id="b524d-215">Маскировать также поддерживает другие механизмы шаблонов, таких как библиотека jQuery.tmpl и Underscore.js в модуль шаблонов.</span><span class="sxs-lookup"><span data-stu-id="b524d-215">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="b524d-216">Компоненты</span><span class="sxs-lookup"><span data-stu-id="b524d-216">Components</span></span>

<span data-ttu-id="b524d-217">Компоненты позволяют упорядочивать и повторно использовать код пользовательского интерфейса, обычно вместе с данными ViewModel, от которого зависит ее код пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="b524d-217">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="b524d-218">Чтобы создать компонент, нужно просто укажите его шаблона и ее viewModel и присвойте ему имя.</span><span class="sxs-lookup"><span data-stu-id="b524d-218">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="b524d-219">Это выполняется с помощью вызова метода `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="b524d-219">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="b524d-220">Кроме определения шаблонов и встроенные viewmodel, их можно загрузить из внешних файлов с помощью библиотеки, такой как *require.js*, что может привести очень простого и эффективного кода.</span><span class="sxs-lookup"><span data-stu-id="b524d-220">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="b524d-221">Взаимодействие с интерфейсами API</span><span class="sxs-lookup"><span data-stu-id="b524d-221">Communicating with APIs</span></span>

<span data-ttu-id="b524d-222">Маскировать можно работать с данными в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="b524d-222">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="b524d-223">Распространенным способом для получения и сохранения данных с помощью маскирования является jQuery, который поддерживает `$.getJSON()` функции для получения данных и `$.post()` метод отправки данных из браузера на конечную точку API.</span><span class="sxs-lookup"><span data-stu-id="b524d-223">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="b524d-224">Конечно при желании способ отправки и получения данных JSON маскирования будет работать с ним также.</span><span class="sxs-lookup"><span data-stu-id="b524d-224">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="b524d-225">Сводка</span><span class="sxs-lookup"><span data-stu-id="b524d-225">Summary</span></span>

<span data-ttu-id="b524d-226">Маскировать обеспечивает простое и элегантное способ привязывать элементы пользовательского интерфейса в текущее состояние клиентского приложения, определенные в ViewModel.</span><span class="sxs-lookup"><span data-stu-id="b524d-226">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="b524d-227">Синтаксис привязки маскирования использует атрибут привязка к данным, применяется к элементам HTML, которые должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="b524d-227">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="b524d-228">Маскирования может эффективно отображения и обновить больших наборов данных, отслеживание элементов пользовательского интерфейса, и только обработкой изменений затронутых элементов.</span><span class="sxs-lookup"><span data-stu-id="b524d-228">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="b524d-229">Большие приложения можно разбить логика пользовательского интерфейса с помощью шаблонов и компонентов, которые могут загружаться по запросу из внешних файлов.</span><span class="sxs-lookup"><span data-stu-id="b524d-229">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="b524d-230">В настоящее время версии 3 маскирования является стабильной библиотека JavaScript, которая может улучшить веб-приложений, в которых требуется взаимодействие клиентских.</span><span class="sxs-lookup"><span data-stu-id="b524d-230">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
