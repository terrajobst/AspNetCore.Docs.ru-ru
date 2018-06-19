---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Использование AJAX для динамического обновления | Документы Microsoft
author: microsoft
description: Шаг 10 реализует поддержку вошедшего в систему пользователям RSVP их интересуют посещение dinner, использование подхода на основе Ajax интегрировано в подробности dinner...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870184"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="0e8e6-103">Использование AJAX для динамического обновления.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="0e8e6-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0e8e6-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0e8e6-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="0e8e6-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="0e8e6-106">Это шаг 10 бесплатных [учебника «Обновление NerdDinner» приложения](introducing-the-nerddinner-tutorial.md) , обходов сквозной как построить небольшой, но завершается веб-приложения с помощью ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="0e8e6-107">Шаг 10 реализует поддержку вошедшего в систему пользователям RSVP их интересуют посещение dinner, использование подхода на основе Ajax интегрировано в странице сведений о компании dinner.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="0e8e6-108">При использовании ASP.NET MVC 3, которому рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="0e8e6-109">Обновление NerdDinner шаг 10: Принимает AJAX Включение ответов на приглашение</span><span class="sxs-lookup"><span data-stu-id="0e8e6-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="0e8e6-110">Теперь рассмотрим процедуру внедрения поддержка RSVP их интересуют посещение dinner вошедших пользователей.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="0e8e6-111">Мы будем обеспечить ее использование подхода на основе AJAX интегрировано в странице сведений о компании dinner.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="0e8e6-112">Указывающее, является ли RSVP'd пользователя</span><span class="sxs-lookup"><span data-stu-id="0e8e6-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="0e8e6-113">Могут посещать пользователи */Dinners/подробности / [идентификатор*] URL-адрес для просмотра сведений о конкретной dinner:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="0e8e6-114">Details(), метод действия реализуется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="0e8e6-115">Наш первый шаг для реализации поддержки RSVP будет добавить вспомогательный метод «IsUserRegistered(username)» объект нашей компании Dinner (внутри разделяемого класса Dinner.cs, созданному ранее).</span><span class="sxs-lookup"><span data-stu-id="0e8e6-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="0e8e6-116">Этот вспомогательный метод возвращает значение true или false в зависимости от того, является ли пользователь в настоящее время RSVP'd для компании Dinner:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="0e8e6-117">Затем можно добавить следующий код в нашей шаблон представление Details.aspx отобразить соответствующее сообщение, указывающее, зарегистрирован ли пользователь или не для события:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="0e8e6-118">И теперь при посещении Dinner, зарегистрированных для их увидите это сообщение:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="0e8e6-119">И при посещении Dinner, они не зарегистрированы для появится следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="0e8e6-120">Реализация метода действия регистра</span><span class="sxs-lookup"><span data-stu-id="0e8e6-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="0e8e6-121">Теперь добавим функциональные возможности, необходимые, чтобы пользователи RSVP на обед на странице сведений о.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="0e8e6-122">Чтобы реализовать это, мы создадим новый класс «RSVPController», щелкнув правой кнопкой мыши каталог \Controllers и выбрав команду Add -&gt;команда меню контроллера.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="0e8e6-123">Реализуется метод действия «Register» в новый класс RSVPController принимает идентификатор на обед в качестве аргумента, получает соответствующий объект Dinner проверяет, если в данный момент в список пользователей, зарегистрированных для него вошедшего в систему пользователя и если не добавляет объект RSVP для них:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="0e8e6-124">Обратите внимание, выше способ возвращение простую строку в качестве выходных данных метода действия.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="0e8e6-125">Нам удалось внедренных в шаблоне представления — это сообщение, но так как оно слишком мало, поэтому мы просто используем вспомогательный метод Content() на базовый класс контроллера и возвращают строковое сообщение, например выше.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="0e8e6-126">Вызов метода действия RSVPForEvent с помощью AJAX</span><span class="sxs-lookup"><span data-stu-id="0e8e6-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="0e8e6-127">Мы будем использовать AJAX для вызова метода действия Register из наших подробного представления.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="0e8e6-128">Такая реализация довольно просто.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="0e8e6-129">Сначала мы добавим двух ссылок на библиотеки скриптов:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="0e8e6-130">Первая библиотека ссылается основной библиотеки ASP.NET AJAX клиентского скрипта.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="0e8e6-131">Этот файл составляет приблизительно 24 КБ размер (сжатые данные) и содержит основные клиентские функции AJAX.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="0e8e6-132">Второй библиотеки содержатся служебные функции, которые интегрируются с ASP.NET MVC встроенных AJAX вспомогательные методы (которые мы будем использовать ближайшее время).</span><span class="sxs-lookup"><span data-stu-id="0e8e6-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="0e8e6-133">Мы можем затем вызова AJAX, вызывает метод действия нашей RSVPForEvent на нашем RSVP контроллер выполняет обновление просмотреть код шаблона, который мы добавили ранее, чтобы вместо outputing сообщение «Вы не зарегистрированы для данного события», мы вместо визуализации ссылки, при передаче и RSVPs пользователя:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="0e8e6-134">Вспомогательный метод Ajax.ActionLink() выше является встроено в ASP.NET MVC и похож на вспомогательный метод Html.ActionLink(), за исключением того, вместо выполнения стандартных навигации он осуществляет вызов AJAX к методу действия при щелчке ссылки.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="0e8e6-135">Выше мы вызова метода действие «Зарегистрировать» на контроллере «RSVP» и передачи DinnerID как параметр «id» в.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="0e8e6-136">Последний параметр AjaxOptions, мы передаем указывает, что мы хотим использовать содержимого, возвращаемого из метода действия и обновите HTML &lt;div&gt; элемента на странице, идентификатор которого — «rsvpmsg».</span><span class="sxs-lookup"><span data-stu-id="0e8e6-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="0e8e6-137">А теперь когда пользователь переходит на обед они не зарегистрированы для, но они будут видеть ссылку RSVP для него:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="0e8e6-138">Если щелкнуть ссылку «RSVP для данного события» они сделать вызов AJAX на метод действия регистрации на контроллере RSVP и после ее завершения, они видят сообщение об обновленных, аналогичные показанным ниже:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="0e8e6-139">Пропускная способность сети и трафика при выполнении этого вызова AJAX действительно невелико.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="0e8e6-140">Когда пользователь щелкает ссылку «RSVP для данного события», небольшой сетевой запрос HTTP POST адресованных */Dinners/Register/1* URL-адрес, который выглядит как ниже по сети:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="0e8e6-141">И ответ от наших метод действия регистрации — это просто:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="0e8e6-142">Этот вызов упрощенных очень быстро и будет работать даже по медленному сетевому подключению.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="0e8e6-143">Добавление jQuery анимации</span><span class="sxs-lookup"><span data-stu-id="0e8e6-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="0e8e6-144">Функциональные возможности AJAX, которые мы реализовали работает хорошо и быстро.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="0e8e6-145">Иногда это может случиться так быстро, что пользователь не заметить, ссылка RSVP заменено новым текстом.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="0e8e6-146">Чтобы сделать результат более очевидным, мы можем добавить простая анимация для привлечения внимания к сообщения обновления.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="0e8e6-147">Значение по умолчанию шаблон проекта ASP.NET MVC включает jQuery — прекрасный (и популярным) открытой библиотека JavaScript, которая также поддерживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="0e8e6-148">jQuery предоставляет ряд функций, включая выбор и эффектов библиотека HTML DOM работы с низким приоритетом.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="0e8e6-149">Для использования jQuery мы сначала добавить ссылку на скрипт на него.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="0e8e6-150">Так как мы будем использовать jQuery в различных мест в сайте, мы добавим ссылку на скрипт в наш файл Site.master главной страницы, чтобы его можно было использовать все страницы.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="0e8e6-151">*Совет: Убедитесь в том, что вы установили исправление JavaScript intellisense для VS 2008 с пакетом обновления 1, обеспечивает более широкую поддержку intellisense для JavaScript файлов (включая jQuery). Его можно загрузить из: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="0e8e6-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="0e8e6-152">Код, написанный с помощью JQuery часто использует глобальный «$ ()» метод JavaScript, который получает один или несколько элементов HTML, с помощью селектора CSS.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="0e8e6-153">Например <em>$("#rsvpmsg")</em> выбирает любой HTML-элемент с идентификатором rsvpmsg, пока <em>$(".something")</em> выбрать все элементы с «текст» CSS имя класса.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="0e8e6-154">Можно также написать более сложные запросы, как «возвращать все отмеченные переключателей» с помощью селектора запрос наподобие: <em>$(«входных данных [@type= переключателей] [@checked]»)</em>.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="0e8e6-155">После выбора элементов для возможность выполнения действий, таких как скрытие их можно вызывать методы: *$(«#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="0e8e6-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="0e8e6-156">В нашем сценарии RSVP мы определим простую функцию JavaScript с именем «AnimateRSVPMessage», выбирающий «rsvpmsg» &lt;div&gt; и выполняет анимацию с размером его содержимого текста.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="0e8e6-157">Ниже кода запускает небольшой текст, а затем вызывает его со периодичностью 400 миллисекунд:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="0e8e6-158">Мы можно затем проводной доступ к этой функции JavaScript, вызываемый после успешного завершения наш вызов AJAX, передав ее имя на нашем вспомогательный метод Ajax.ActionLink() (через AjaxOptions «OnSuccess» свойства события):</span><span class="sxs-lookup"><span data-stu-id="0e8e6-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="0e8e6-159">И теперь при нажатии ссылки «RSVP для этого события» и наши вызов AJAX успешного завершения содержимого сообщения, отправленного назад будет анимации и увеличению:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="0e8e6-160">Помимо события «OnSuccess», объект AjaxOptions предоставляет методы OnBegin, OnFailure и OnComplete события, которые можно обрабатывать (а также ряд других свойств и полезные параметры).</span><span class="sxs-lookup"><span data-stu-id="0e8e6-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="0e8e6-161">Очистка - рефакторинга out RSVP частичного представления</span><span class="sxs-lookup"><span data-stu-id="0e8e6-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="0e8e6-162">Наш шаблон представления сведений начинает получить немного long, какие сверхурочной работы затруднит немного для понимания.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="0e8e6-163">Чтобы сделать код более понятным, добавим путем создания частичное представление — RSVPStatus.ascx —, инкапсулирующими все представления кода RSVP нашу страницу сведений.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="0e8e6-164">Это можно сделать, щелкнув правой кнопкой мыши по папке \Views\Dinners, а затем выберите Add -&gt;просмотреть команды меню.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="0e8e6-165">У нас будет их в качестве ее ViewModel строго типизированный объект обед.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="0e8e6-166">Мы можно затем скопируйте и вставьте содержимое RSVP из наших представление Details.aspx, в него.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="0e8e6-167">Если мы выполнили, также создадим другой частичного представления — EditAndDeleteLinks.ascx -, инкапсулирующий наш код представление связи Edit и Delete.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="0e8e6-168">Мы также получите его принимают в качестве ее ViewModel строго типизированный объект Dinner и скопируйте логики Edit и Delete из наших представление Details.aspx, в него.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="0e8e6-169">Подробную информацию шаблона можно просмотреть, а затем просто содержат два вызова метода Html.RenderPartial() внизу:</span><span class="sxs-lookup"><span data-stu-id="0e8e6-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="0e8e6-170">Это делает код очистки для понимания и обслуживания.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="0e8e6-171">Следующий шаг</span><span class="sxs-lookup"><span data-stu-id="0e8e6-171">Next Step</span></span>

<span data-ttu-id="0e8e6-172">Давайте теперь взглянем на как можно еще более используется AJAX и добавить поддержку интерактивного сопоставления для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="0e8e6-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e8e6-173">[Назад](secure-applications-using-authentication-and-authorization.md)
> [Вперед](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="0e8e6-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
