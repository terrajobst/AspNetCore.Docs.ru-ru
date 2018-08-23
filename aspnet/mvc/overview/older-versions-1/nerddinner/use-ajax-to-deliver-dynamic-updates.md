---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Использование AJAX для доставки динамических обновлений | Документация Майкрософт
author: microsoft
description: Шаг 10 реализует поддержку вошедшего в систему пользователям RSVP их интерес к конференции обед, с помощью подхода на основе Ajax, интегрируются в подробные сведения о компании dinner...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: e902881d3dab6a902cb747a197a32f317d199723
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829020"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="86b54-103">Использование AJAX для доставки динамических обновлений</span><span class="sxs-lookup"><span data-stu-id="86b54-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="86b54-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="86b54-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="86b54-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="86b54-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="86b54-106">Это шаг 10 процедуры бесплатной [руководство по использованию приложения «NerdDinner»](introducing-the-nerddinner-tutorial.md) , пошаговое рассмотрение как создать небольшой, но завершить, веб-приложения с помощью ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="86b54-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="86b54-107">Шаг 10 реализует поддержку вошедшего в систему пользователям RSVP их интерес к конференции обед, с помощью подхода на основе Ajax, интегрируются в странице сведений о компании dinner.</span><span class="sxs-lookup"><span data-stu-id="86b54-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="86b54-108">Если вы используете ASP.NET MVC 3, рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="86b54-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="86b54-109">NerdDinner шаг 10: Принимает AJAX Включение Кроме</span><span class="sxs-lookup"><span data-stu-id="86b54-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="86b54-110">Давайте теперь реализация поддержки для вошедшего в систему пользователям RSVP их интерес к конференции ужин.</span><span class="sxs-lookup"><span data-stu-id="86b54-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="86b54-111">Мы расширим это с помощью интегрируются в странице сведений о компании dinner подхода на основе AJAX.</span><span class="sxs-lookup"><span data-stu-id="86b54-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="86b54-112">Указывающее, является ли RSVP'd пользователя</span><span class="sxs-lookup"><span data-stu-id="86b54-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="86b54-113">Пользователи смогут посещать */Dinners/сведения / [идентификатор*] URL-адрес, чтобы просмотреть сведения о примерную стоимость:</span><span class="sxs-lookup"><span data-stu-id="86b54-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="86b54-114">Details(), метод действия реализуется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86b54-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="86b54-115">Нашим первым этапом для реализации поддержки RSVP будет добавьте вспомогательный метод «IsUserRegistered(username)» для нашей компании Dinner (в пределах Dinner.cs разделяемый класс, который мы создали ранее).</span><span class="sxs-lookup"><span data-stu-id="86b54-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="86b54-116">Этот вспомогательный метод возвращает значение true или false в зависимости от того, является ли пользователь в настоящее время RSVP'd для компании Dinner:</span><span class="sxs-lookup"><span data-stu-id="86b54-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="86b54-117">Затем мы добавляем следующий код к шаблону представление Details.aspx для отображения соответствующее сообщение, указывающее, зарегистрирован ли пользователь или не для события:</span><span class="sxs-lookup"><span data-stu-id="86b54-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="86b54-118">И теперь при посещении обед, они зарегистрированы для появится это сообщение:</span><span class="sxs-lookup"><span data-stu-id="86b54-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="86b54-119">И при посещении обед, они не зарегистрированы для он увидит следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="86b54-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="86b54-120">Реализация метода Register действия</span><span class="sxs-lookup"><span data-stu-id="86b54-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="86b54-121">Теперь добавим функциональные возможности, необходимые, чтобы пользователи RSVP на обед на странице сведений.</span><span class="sxs-lookup"><span data-stu-id="86b54-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="86b54-122">Чтобы реализовать это, мы создадим новый класс «RSVPController», щелкнув правой кнопкой мыши каталог \Controllers и выбрав Add -&gt;контроллера меню команды.</span><span class="sxs-lookup"><span data-stu-id="86b54-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="86b54-123">Мы реализуем метод действие «Зарегистрировать» в новый класс RSVPController, который принимает идентификатор на ужин в качестве аргумента, извлекает соответствующий объект Dinner, проверяет, вошедшего в систему пользователя находится в списке пользователей, зарегистрированных для него, если не добавляет объект RSVP для них:</span><span class="sxs-lookup"><span data-stu-id="86b54-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="86b54-124">Обратите внимание, как мы возвращается простая строка как выходные данные метода действия.</span><span class="sxs-lookup"><span data-stu-id="86b54-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="86b54-125">Мы удалось встроенным это сообщение в шаблоне представления —, но так как это настолько малы, просто используем вспомогательный метод Content() на базовый класс контроллера и возвращают строковое сообщение, например выше.</span><span class="sxs-lookup"><span data-stu-id="86b54-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="86b54-126">Вызов метода RSVPForEvent действия, с помощью AJAX</span><span class="sxs-lookup"><span data-stu-id="86b54-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="86b54-127">Мы будем использовать AJAX для вызова метода действия Register из нашего представления сведений.</span><span class="sxs-lookup"><span data-stu-id="86b54-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="86b54-128">Такая реализация довольно проста.</span><span class="sxs-lookup"><span data-stu-id="86b54-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="86b54-129">Сначала мы добавим две ссылки на библиотеку скрипта:</span><span class="sxs-lookup"><span data-stu-id="86b54-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="86b54-130">Первая библиотека ссылается на основной библиотеки клиентского скрипта ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="86b54-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="86b54-131">Этот файл составляет приблизительно 24 КБ в размере (со сжатием) и содержит основные функциональные возможности AJAX на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="86b54-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="86b54-132">Второй библиотеки содержит служебные функции, которые интегрируются с ASP.NET MVC встроенные AJAX вспомогательные методы (которые мы будем использовать чуть позже).</span><span class="sxs-lookup"><span data-stu-id="86b54-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="86b54-133">Мы можем затем обновление код шаблона представления, реализованной ранее, чтобы вместо выводе сообщение «Вы не зарегистрированы для данного события», мы вместо визуализации ссылки, при отправке выполняет вызов AJAX, вызывает метод действия наших RSVPForEvent в нашем контроллере RSVP и RSVPs пользователя:</span><span class="sxs-lookup"><span data-stu-id="86b54-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="86b54-134">Вспомогательный метод Ajax.ActionLink() выше встроенных в ASP.NET MVC так же и вспомогательного метода Html.ActionLink() за исключением того, что вместо выполнения стандартный навигационный он осуществляет вызов AJAX на метод действия, при щелчке ссылки.</span><span class="sxs-lookup"><span data-stu-id="86b54-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="86b54-135">Выше мы вызовом метода действия «Зарегистрировать» на контроллере «RSVP» и передавая DinnerID как параметр «id», к нему.</span><span class="sxs-lookup"><span data-stu-id="86b54-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="86b54-136">Последний параметр AjaxOptions, мы передаем указывает, что мы хотим получить содержимое, возвращаемое из метода действия и обновите HTML &lt;div&gt; элемента на странице, идентификатор которого — «rsvpmsg».</span><span class="sxs-lookup"><span data-stu-id="86b54-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="86b54-137">И теперь когда пользователь переходит на обед они не зарегистрированы для, но он увидит ссылку RSVP для него:</span><span class="sxs-lookup"><span data-stu-id="86b54-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="86b54-138">При переходе по ссылке «RSVP для этого события», они должны быть вызов AJAX на метод действия регистрации на контроллере RSVP, и после ее завершения, он увидит сообщение об обновленных следующее:</span><span class="sxs-lookup"><span data-stu-id="86b54-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="86b54-139">Пропускная способность сети и трафика, задействованные при этом вызов AJAX действительно невелико.</span><span class="sxs-lookup"><span data-stu-id="86b54-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="86b54-140">Выполняется при нажатии на ссылку «RSVP для этого события», небольшой сетевой запрос HTTP POST для */Dinners/Register/1* URL-адрес, который выглядит как ниже по сети:</span><span class="sxs-lookup"><span data-stu-id="86b54-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="86b54-141">И ответ от нашего метода Register действие — это просто:</span><span class="sxs-lookup"><span data-stu-id="86b54-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="86b54-142">Этот упрощенный вызов выполняется быстро и будет работать даже по медленной сети.</span><span class="sxs-lookup"><span data-stu-id="86b54-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="86b54-143">Добавление анимации jQuery</span><span class="sxs-lookup"><span data-stu-id="86b54-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="86b54-144">Функциональные возможности AJAX, которые мы реализовали работает правильно и быстро.</span><span class="sxs-lookup"><span data-stu-id="86b54-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="86b54-145">Иногда это может случиться так быстро, что пользователь не заметить, что ссылка RSVP был заменен новым текстом.</span><span class="sxs-lookup"><span data-stu-id="86b54-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="86b54-146">Чтобы сделать результат немного очевиднее, мы можем добавить простой анимации для привлечения внимания к сообщению обновления.</span><span class="sxs-lookup"><span data-stu-id="86b54-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="86b54-147">По умолчанию шаблон проекта ASP.NET MVC включает в себя jQuery – это библиотека JavaScript высокими показателями (и очень популярным) открытым кодом, который также поддерживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="86b54-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="86b54-148">jQuery предоставляет ряд функций, включая выделение и эффекты библиотека HTML DOM неплохо.</span><span class="sxs-lookup"><span data-stu-id="86b54-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="86b54-149">Для использования jQuery сначала добавим ссылку сценария на него.</span><span class="sxs-lookup"><span data-stu-id="86b54-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="86b54-150">Так как мы собираемся использовать jQuery в различных местах в наш сайт, мы добавим ссылку на сценарий в наш файл Site.master главной страницы, чтобы все страницы можно использовать.</span><span class="sxs-lookup"><span data-stu-id="86b54-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="86b54-151">*Совет: Убедитесь, что вы установили исправление JavaScript intellisense для VS 2008 SP1, позволяющий более обширная поддержка intellisense файлы JavaScript (включая jQuery). Его можно загрузить из: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="86b54-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="86b54-152">Код, написанный с помощью JQuery, часто использует глобальный «$ ()» метод JavaScript, который получает один или несколько элементов HTML, с помощью селектора CSS.</span><span class="sxs-lookup"><span data-stu-id="86b54-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="86b54-153">Например <em>$("#rsvpmsg")</em> выбирает любой HTML-элемент с идентификатором rsvpmsg, хотя <em>$(".something")</em> следует выбрать все элементы с «что-то» CSS имя класса.</span><span class="sxs-lookup"><span data-stu-id="86b54-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="86b54-154">Можно также написать более сложные запросы, как «return все кнопки установлен переключатель» с помощью селектора запроса как: <em>$("входные данные [@type= radio] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="86b54-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="86b54-155">После выбора элементов, можно вызывать методы на возможность выполнения действий, таких как скрытие их: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="86b54-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="86b54-156">В нашем сценарии RSVP мы определим простой функции JavaScript с именем «AnimateRSVPMessage», который выбирает «rsvpmsg» &lt;div&gt; и анимируется размер его текстовое содержимое.</span><span class="sxs-lookup"><span data-stu-id="86b54-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="86b54-157">Ниже кода запускает небольшой текст, а затем вызывает его, чтобы увеличить за период времени, 400 миллисекунд:</span><span class="sxs-lookup"><span data-stu-id="86b54-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="86b54-158">Мы можно затем проводной доступ к этой функции JavaScript, вызываемый после успешного выполнения наш вызов AJAX, передав ее имя на наших Ajax.ActionLink() вспомогательный метод (с помощью AjaxOptions «OnSuccess» свойства события):</span><span class="sxs-lookup"><span data-stu-id="86b54-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="86b54-159">А теперь нажатии ссылки «RSVP для этого события» и наш вызов AJAX успешном завершении, содержимого сообщения, отправленного назад будет анимация и увеличению:</span><span class="sxs-lookup"><span data-stu-id="86b54-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="86b54-160">Помимо «OnSuccess» событие, объект AjaxOptions предоставляет методы OnBegin OnFailure и OnComplete события, которые можно обработать (а также ряд других свойств и полезные параметры).</span><span class="sxs-lookup"><span data-stu-id="86b54-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="86b54-161">Очистка — рефакторинг out RSVP частичного представления</span><span class="sxs-lookup"><span data-stu-id="86b54-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="86b54-162">Наши шаблона представления сведений о начинает получить немного long, какие сверхурочные поможет вам чуть сложнее для понимания.</span><span class="sxs-lookup"><span data-stu-id="86b54-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="86b54-163">Чтобы улучшить читаемость кода, давайте закончить путем создания частичное представление — RSVPStatus.ascx —, инкапсулировать весь код RSVP представление страницы сведений.</span><span class="sxs-lookup"><span data-stu-id="86b54-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="86b54-164">Это можно сделать, щелкнув правой кнопкой мыши на папку \Views\Dinners и выбрав Add -&gt;просмотреть команду меню.</span><span class="sxs-lookup"><span data-stu-id="86b54-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="86b54-165">У нас будет их в качестве его модель представления, строго типизированный объект ужин.</span><span class="sxs-lookup"><span data-stu-id="86b54-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="86b54-166">Мы можно затем скопируйте и вставьте содержимое RSVP из нашего представления Details.aspx в него.</span><span class="sxs-lookup"><span data-stu-id="86b54-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="86b54-167">Как только мы этого также создадим другой частичное представление — EditAndDeleteLinks.ascx - инкапсулируемого наш код представления ссылку Edit и Delete.</span><span class="sxs-lookup"><span data-stu-id="86b54-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="86b54-168">Кроме того, состоится принимать в качестве его модель представления, строго типизированный объект Dinner, а также копирования и вставки из нашего представления Details.aspx в него логику изменение и удаление.</span><span class="sxs-lookup"><span data-stu-id="86b54-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="86b54-169">Подробную информацию просмотреть шаблоны, а затем просто включают два вызова метода Html.RenderPartial() внизу:</span><span class="sxs-lookup"><span data-stu-id="86b54-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="86b54-170">Это делает код чище, чтение и обслуживание.</span><span class="sxs-lookup"><span data-stu-id="86b54-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="86b54-171">Следующий шаг</span><span class="sxs-lookup"><span data-stu-id="86b54-171">Next Step</span></span>

<span data-ttu-id="86b54-172">Давайте теперь взглянем на как можно использовать AJAX еще дальше и добавить поддержку интерактивных топографических к нашему приложению.</span><span class="sxs-lookup"><span data-stu-id="86b54-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86b54-173">[Назад](secure-applications-using-authentication-and-authorization.md)
> [Вперед](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="86b54-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
