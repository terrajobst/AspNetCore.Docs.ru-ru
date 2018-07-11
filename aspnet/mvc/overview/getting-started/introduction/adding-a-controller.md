---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Добавление контроллера | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38119267"
---
<a name="adding-a-controller"></a><span data-ttu-id="6b119-102">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="6b119-102">Adding a Controller</span></span>
====================
<span data-ttu-id="6b119-103">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6b119-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="6b119-104">MVC означает *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="6b119-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="6b119-105">MVC — это шаблон для разработки приложений, которые хорошо продуманной архитектурой, пригодного для тестирования и простые в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="6b119-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="6b119-106">Приложения на основе MVC содержат:</span><span class="sxs-lookup"><span data-stu-id="6b119-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="6b119-107">**M** odels: классы, представляющие данные приложения и используют логику проверки для реализации бизнес-правил к этим данным.</span><span class="sxs-lookup"><span data-stu-id="6b119-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="6b119-108">**V** редставления: файлы шаблонов, которые приложение использует для динамического создания HTML-ответов.</span><span class="sxs-lookup"><span data-stu-id="6b119-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="6b119-109">**C** ontrollers: классы, которые обрабатывают входящие запросы браузера, извлекают данные модели, а затем укажите шаблоны представлений, которые возвращают ответ в браузере.</span><span class="sxs-lookup"><span data-stu-id="6b119-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="6b119-110">Мы будет обсуждаться в этой серии руководств, все эти концепции и показать, как их использовать для создания приложения.</span><span class="sxs-lookup"><span data-stu-id="6b119-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="6b119-111">Шаблон был выбран на предыдущем шаге MVC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6b119-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="6b119-112">При этом создается главная, учетная запись и управление контроллерами, по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6b119-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="6b119-113">Начнем с создания класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b119-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="6b119-114">В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папку и нажмите кнопку **добавить**, затем **контроллера**.</span><span class="sxs-lookup"><span data-stu-id="6b119-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="6b119-115">В **Добавление шаблона** диалоговом окне щелкните **контроллер MVC 5 — пустой**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="6b119-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="6b119-116">Назовите новый контроллер «HelloWorldController» и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="6b119-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Добавление контроллера](adding-a-controller/_static/image3.png)

<span data-ttu-id="6b119-118">Обратите внимание, что в **обозревателе решений** что новый файл был создан именованный *HelloWorldController.cs* и ее *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="6b119-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="6b119-119">Контроллер открыт в интегрированной среде разработки.</span><span class="sxs-lookup"><span data-stu-id="6b119-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="6b119-120">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="6b119-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="6b119-121">Методы контроллера будет возвращена строка HTML, в качестве примера.</span><span class="sxs-lookup"><span data-stu-id="6b119-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="6b119-122">Контроллер называется `HelloWorldController` и первый метод называется `Index`.</span><span class="sxs-lookup"><span data-stu-id="6b119-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="6b119-123">Давайте вызвать его из браузера.</span><span class="sxs-lookup"><span data-stu-id="6b119-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="6b119-124">Запустите приложение (нажмите клавишу F5 или Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="6b119-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="6b119-125">В браузере, добавьте &quot;HelloWorld&quot; к пути в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="6b119-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="6b119-126">(Например, на рисунке ниже, его `http://localhost:1234/HelloWorld.`) страница в браузере будет выглядеть как на следующем снимке экрана.</span><span class="sxs-lookup"><span data-stu-id="6b119-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="6b119-127">В методе выше код вернул строку непосредственно.</span><span class="sxs-lookup"><span data-stu-id="6b119-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="6b119-128">Вы сказали, что система должна возвращать только некоторые HTML, как и!</span><span class="sxs-lookup"><span data-stu-id="6b119-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="6b119-129">ASP.NET MVC вызывает классы другой контроллер (и различных методов действий в них), в зависимости от входящего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="6b119-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="6b119-130">Логику маршрутизации URL-адрес по умолчанию используется ASP.NET MVC использует формат следующим образом, чтобы определить, какой код для вызова:</span><span class="sxs-lookup"><span data-stu-id="6b119-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="6b119-131">Формат маршрутизации задается *приложения\_Start/RouteConfig.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="6b119-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="6b119-132">При запуске приложения и не указывайте все сегменты URL-адреса, по умолчанию используется «Home» контроллера и действия метод «Index», заданный в раздел defaults приведенный выше код.</span><span class="sxs-lookup"><span data-stu-id="6b119-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="6b119-133">Первая часть URL-адреса определяет класс контроллера для выполнения.</span><span class="sxs-lookup"><span data-stu-id="6b119-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="6b119-134">Поэтому */HelloWorld* сопоставляется `HelloWorldController` класса.</span><span class="sxs-lookup"><span data-stu-id="6b119-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="6b119-135">Вторая часть URL-адреса определяет метод действия к классу для выполнения.</span><span class="sxs-lookup"><span data-stu-id="6b119-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="6b119-136">Поэтому */HelloWorld/индекс* вызовет `Index` метод `HelloWorldController` класс для выполнения.</span><span class="sxs-lookup"><span data-stu-id="6b119-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="6b119-137">Обратите внимание, что нам пришлось перейдите к */HelloWorld* и `Index` метод использовался по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6b119-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="6b119-138">Это обусловлено тем, метод с именем `Index` является методом по умолчанию, который будет вызываться на контроллере, если тип не задан явным образом.</span><span class="sxs-lookup"><span data-stu-id="6b119-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="6b119-139">В третьей части сегмента URL-адреса (`Parameters`) указываются данные маршрута.</span><span class="sxs-lookup"><span data-stu-id="6b119-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="6b119-140">Данные маршрута будет показано позже в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="6b119-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="6b119-141">Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="6b119-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="6b119-142">`Welcome` Метод запускается и возвращает строку &quot;метод приветствия действие... &quot;.</span><span class="sxs-lookup"><span data-stu-id="6b119-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="6b119-143">MVC по умолчанию осуществляется сопоставление `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="6b119-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="6b119-144">Для этого URL-адреса заданы контроллер `HelloWorld` и метод действия `Welcome`.</span><span class="sxs-lookup"><span data-stu-id="6b119-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="6b119-145">Часть URL-адреса `[Parameters]` на данный момент еще не использовалась.</span><span class="sxs-lookup"><span data-stu-id="6b119-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="6b119-146">Давайте немного изменить приведенный пример, чтобы передать сведения о параметрах из URL-адрес в контроллер (например, */HelloWorld/Welcome? имя = Скотт&amp;; numtimes = = 4*).</span><span class="sxs-lookup"><span data-stu-id="6b119-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="6b119-147">Изменение вашего `Welcome` метод включив два параметра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6b119-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="6b119-148">Обратите внимание, что код использует функцию необязательного параметра C#, чтобы указать, что `numTimes` параметр должен по умолчанию 1, если не передаются значения для этого параметра.</span><span class="sxs-lookup"><span data-stu-id="6b119-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="6b119-149">Примечание по безопасности: Приведенном выше примере используется [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) для защиты приложения от злонамеренного ввода данных (JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6b119-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="6b119-150">Дополнительные сведения см. в разделе [как: защита от скриптов в веб-приложении внедрения в строки](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="6b119-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="6b119-151">Запустите приложение и перейдите к пример URL-адреса (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="6b119-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="6b119-152">Вы можете попробовать различные значения для `name` и `numtimes` в URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="6b119-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="6b119-153">[Система привязки модели ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) автоматически сопоставляет именованные параметры из строки запроса в адресной строке параметров метода.</span><span class="sxs-lookup"><span data-stu-id="6b119-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="6b119-154">В примере выше, сегмент URL-адреса ( `Parameters`) не используется, `name` и `numTimes` параметры передаются как [строки запроса](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="6b119-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="6b119-155">Подстановочный знак ?</span><span class="sxs-lookup"><span data-stu-id="6b119-155">The ?</span></span> <span data-ttu-id="6b119-156">(вопросительный знак) в приведенном выше URL-адрес является разделителем и указываются строки запроса.</span><span class="sxs-lookup"><span data-stu-id="6b119-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="6b119-157">Символ &amp; разделяет строки запроса.</span><span class="sxs-lookup"><span data-stu-id="6b119-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="6b119-158">Замените метод приветствия следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6b119-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="6b119-159">Запустите приложение и введите следующий URL-адрес: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="6b119-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="6b119-160">Этот раз третий сегмент URL-адрес сопоставляется с параметром маршрута `ID.` `Welcome` метод действия содержит параметр (`ID`), соответствующих спецификации URL-адрес в `RegisterRoutes` метод.</span><span class="sxs-lookup"><span data-stu-id="6b119-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="6b119-161">В приложениях ASP.NET MVC обычно передавать параметры в качестве данных маршрута (как это делалось с Идентификатором выше), чем передавать их в качестве строки запроса.</span><span class="sxs-lookup"><span data-stu-id="6b119-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="6b119-162">Можно также добавить маршрут для передачи оба `name` и `numtimes` в параметры в качестве данных маршрута в URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="6b119-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="6b119-163">В *приложения\_Start\RouteConfig.cs* добавьте маршрут «Hello»:</span><span class="sxs-lookup"><span data-stu-id="6b119-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="6b119-164">Запустите приложение и перейдите к `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="6b119-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="6b119-165">Для многих приложений MVC маршрут по умолчанию работает нормально.</span><span class="sxs-lookup"><span data-stu-id="6b119-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="6b119-166">Вы узнаете далее в этом руководстве для передачи данных с помощью связывателя модели, и вам не придется изменить маршрут по умолчанию для этого.</span><span class="sxs-lookup"><span data-stu-id="6b119-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="6b119-167">В этих примерах контроллер предоставляет &quot;VC&quot; модели MVC — то есть работу представления и контроллера.</span><span class="sxs-lookup"><span data-stu-id="6b119-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="6b119-168">Контроллер возвращает HTML напрямую.</span><span class="sxs-lookup"><span data-stu-id="6b119-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="6b119-169">Обычно это не требуется Возвращает HTML напрямую, поскольку в этом случае заметно усложняется написание кода.</span><span class="sxs-lookup"><span data-stu-id="6b119-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="6b119-170">Вместо этого мы обычно используем файл шаблона отдельное представление для создания HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="6b119-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="6b119-171">Давайте взглянем далее в том, как это можно сделать.</span><span class="sxs-lookup"><span data-stu-id="6b119-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6b119-172">[Назад](getting-started.md)
> [Вперед](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="6b119-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
