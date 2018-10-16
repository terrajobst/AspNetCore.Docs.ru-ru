---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Включение запросов о происхождении в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: Показано, как для поддержки общего доступа к независимо от источника (CORS) в веб-API ASP.NET.
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348524"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="29966-103">Включение запросов о происхождении в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="29966-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="29966-104">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="29966-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="29966-105">Безопасность обозревателя запрещает отправку запросов AJAX в другой домен веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="29966-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="29966-106">Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="29966-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="29966-107">Тем не менее иногда вам может потребоваться разрешить другие сайты вызова веб-API.</span><span class="sxs-lookup"><span data-stu-id="29966-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="29966-108">[Кросс-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) — это стандарт консорциума W3C, позволяющий серверу смягчить ограничения политики одного источника.</span><span class="sxs-lookup"><span data-stu-id="29966-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="29966-109">С помощью CORS сервер может явным образом разрешить некоторые запросы независимо от источника а другие — отклонять.</span><span class="sxs-lookup"><span data-stu-id="29966-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="29966-110">CORS — более безопасное и более гибким, чем предыдущие технологии, такие как [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="29966-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="29966-111">Этом руководстве показано, как включить поддержку CORS в приложении веб-API.</span><span class="sxs-lookup"><span data-stu-id="29966-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="29966-112">Программное обеспечение, используемое в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="29966-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="29966-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29966-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="29966-114">Веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="29966-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="29966-115">Вступление</span><span class="sxs-lookup"><span data-stu-id="29966-115">Introduction</span></span>

<span data-ttu-id="29966-116">В этом учебнике показано, что поддержка CORS в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="29966-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="29966-117">Мы начнем с создания двух проектов ASP.NET — один вызываемой «веб-служба», где размещен контроллер Web API, и другие вызываемой «WebClient», который вызывает веб-службы.</span><span class="sxs-lookup"><span data-stu-id="29966-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="29966-118">Так как оба приложения размещены в разных доменах, AJAX-запрос из веб-клиента для веб-службы — это запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="29966-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="29966-119">Что такое «того же происхождения»?</span><span class="sxs-lookup"><span data-stu-id="29966-119">What is "same origin"?</span></span>

<span data-ttu-id="29966-120">Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты.</span><span class="sxs-lookup"><span data-stu-id="29966-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="29966-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="29966-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="29966-122">Эти два URL-адреса у того же происхождения:</span><span class="sxs-lookup"><span data-stu-id="29966-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="29966-123">Эти URL-адреса имеют различное происхождение по сравнению с предыдущим два:</span><span class="sxs-lookup"><span data-stu-id="29966-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="29966-124">`http://example.net` -Другой домен</span><span class="sxs-lookup"><span data-stu-id="29966-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="29966-125">`http://example.com:9000/foo.html` -Другой порт</span><span class="sxs-lookup"><span data-stu-id="29966-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="29966-126">`https://example.com/foo.html` -Другую схему</span><span class="sxs-lookup"><span data-stu-id="29966-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="29966-127">`http://www.example.com/foo.html` -Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="29966-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="29966-128">Internet Explorer не учитывает порт при сравнении источников.</span><span class="sxs-lookup"><span data-stu-id="29966-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="29966-129">Создание проекта веб-службы</span><span class="sxs-lookup"><span data-stu-id="29966-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="29966-130">В этом разделе предполагается, что вы уже умеете создавать проекты веб-API.</span><span class="sxs-lookup"><span data-stu-id="29966-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="29966-131">Если это не так, см. в разделе [Приступая к работе с веб-API ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="29966-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="29966-132">Запустите Visual Studio и создайте новый **веб-приложение ASP.NET (.NET Framework)** проекта.</span><span class="sxs-lookup"><span data-stu-id="29966-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="29966-133">В **новое веб-приложение ASP.NET** выберите **пустой** шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="29966-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="29966-134">В разделе **добавить папки и основные ссылки для**выберите **веб-API** флажок.</span><span class="sxs-lookup"><span data-stu-id="29966-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![ASP.NET диалоговое окно нового проекта в Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="29966-136">Добавление контроллера веб-API с именем `TestController` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="29966-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="29966-137">Можно запустить приложение локально или развертывать в Azure.</span><span class="sxs-lookup"><span data-stu-id="29966-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="29966-138">(Снимки экрана, в этом руководстве, приложение развертывается в веб-приложениях службы приложений Azure.) Чтобы убедиться, что веб-API работает, перейдите к `http://hostname/api/test/`, где *hostname* — это домен, в котором развертывается приложение.</span><span class="sxs-lookup"><span data-stu-id="29966-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="29966-139">Вы должны увидеть текст ответа, &quot;получить: тестовое сообщение&quot;.</span><span class="sxs-lookup"><span data-stu-id="29966-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Web браузера отображение тестовое сообщение](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="29966-141">Создание проекта веб-клиента</span><span class="sxs-lookup"><span data-stu-id="29966-141">Create the WebClient project</span></span>

1. <span data-ttu-id="29966-142">Создайте другой **веб-приложение ASP.NET (.NET Framework)** проекта и выберите **MVC** шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="29966-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="29966-143">При необходимости выберите **изменить способ проверки подлинности** > **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="29966-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="29966-144">Не требуется проверка подлинности для этого руководства.</span><span class="sxs-lookup"><span data-stu-id="29966-144">You don't need authentication for this tutorial.</span></span>

   ![Шаблон MVC в диалоговое окно Новый проект ASP.NET в Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="29966-146">В **обозревателе решений**, откройте файл *Views/Home/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="29966-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="29966-147">Замените код в этот файл следующее:</span><span class="sxs-lookup"><span data-stu-id="29966-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="29966-148">Для *serviceUrl* переменной, используйте URI веб-службы приложения.</span><span class="sxs-lookup"><span data-stu-id="29966-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="29966-149">Запустите приложение WebClient локально или опубликовать его в другой веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="29966-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="29966-150">При нажатии кнопки «Попробовать», AJAX-запрос отправляется в приложение веб-службы, с помощью метода HTTP, перечисленные в раскрывающемся списке (GET, POST или PUT).</span><span class="sxs-lookup"><span data-stu-id="29966-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="29966-151">Это позволяет просматривать различные запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="29966-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="29966-152">В настоящее время приложение веб-службы не поддерживает CORS, поэтому если нажать кнопку, вы получите ошибку.</span><span class="sxs-lookup"><span data-stu-id="29966-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Ошибка «Попробуйте» в браузере](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="29966-154">Если смотреть HTTP-трафика в средстве, например [Fiddler](http://www.telerik.com/fiddler), вы увидите, что браузер отправляет запрос GET и запрос выполнен успешно, но вызов AJAX возвращает ошибку.</span><span class="sxs-lookup"><span data-stu-id="29966-154">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="29966-155">Важно понимать, что политика одного источника не запрещает браузере из *отправки* запроса.</span><span class="sxs-lookup"><span data-stu-id="29966-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="29966-156">Вместо этого он дает приложению просматривать *ответа*.</span><span class="sxs-lookup"><span data-stu-id="29966-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Веб-отладчик Fiddler, показывающая веб-запросов](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="29966-158">Включение CORS</span><span class="sxs-lookup"><span data-stu-id="29966-158">Enable CORS</span></span>

<span data-ttu-id="29966-159">Теперь давайте Включение CORS в приложении веб-службы.</span><span class="sxs-lookup"><span data-stu-id="29966-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="29966-160">Во-первых добавьте пакет CORS NuGet.</span><span class="sxs-lookup"><span data-stu-id="29966-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="29966-161">В Visual Studio из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="29966-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="29966-162">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29966-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="29966-163">Эта команда устанавливает последнюю версию пакета и обновляет все зависимости, включая библиотеки core веб-API.</span><span class="sxs-lookup"><span data-stu-id="29966-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="29966-164">Используйте `-Version` флаг, чтобы использовать конкретную версию.</span><span class="sxs-lookup"><span data-stu-id="29966-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="29966-165">CORS пакету требуется веб-API 2.0 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="29966-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="29966-166">Откройте файл *приложения\_Start/WebApiConfig.cs*.</span><span class="sxs-lookup"><span data-stu-id="29966-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="29966-167">Добавьте следующий код, чтобы **WebApiConfig.Register** метод:</span><span class="sxs-lookup"><span data-stu-id="29966-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="29966-168">Затем добавьте **[EnableCors]** атрибут `TestController` класса:</span><span class="sxs-lookup"><span data-stu-id="29966-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="29966-169">Для *источников* параметра, используйте URI, на котором развертывается приложение WebClient.</span><span class="sxs-lookup"><span data-stu-id="29966-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="29966-170">Это позволяет запросов о происхождении из WebClient, по-прежнему запрета на все остальные запросы между доменами.</span><span class="sxs-lookup"><span data-stu-id="29966-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="29966-171">Позже я расскажу о параметры **[EnableCors]** более подробно.</span><span class="sxs-lookup"><span data-stu-id="29966-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="29966-172">Не используйте косую черту в конце *источников* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="29966-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="29966-173">Повторное развертывание обновленного приложения веб-службы.</span><span class="sxs-lookup"><span data-stu-id="29966-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="29966-174">Не нужно обновлять WebClient.</span><span class="sxs-lookup"><span data-stu-id="29966-174">You don't need to update WebClient.</span></span> <span data-ttu-id="29966-175">Теперь запрос AJAX из WebClient должны завершиться успешно.</span><span class="sxs-lookup"><span data-stu-id="29966-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="29966-176">Методы GET, PUT и POST все разрешены.</span><span class="sxs-lookup"><span data-stu-id="29966-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Отображение успешного завершения проверки Web браузера сообщения](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="29966-178">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="29966-178">How CORS Works</span></span>

<span data-ttu-id="29966-179">В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="29966-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="29966-180">Очень важно понять, как работает CORS, таким образом, можно настроить **[EnableCors]** атрибут правильно и устранить неполадки, если не все работает как надо.</span><span class="sxs-lookup"><span data-stu-id="29966-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="29966-181">Спецификация CORS представляет ряд новых заголовков HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="29966-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="29966-182">Если браузер поддерживает CORS, он устанавливает эти заголовки автоматически для запросов о происхождении; не нужно предпринимать специальные действия в коде JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29966-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="29966-183">Вот пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="29966-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="29966-184">Заголовка «Origin» предоставляет домену узла, который выполняет запрос.</span><span class="sxs-lookup"><span data-stu-id="29966-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="29966-185">Если сервер разрешает запрос, он задает заголовка Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="29966-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="29966-186">Значение этого заголовка соответствует заголовку источника либо является использование подстановочного знака "\*«, это значит, что допускается любого источника.</span><span class="sxs-lookup"><span data-stu-id="29966-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="29966-187">Если ответ не содержит заголовка Access-Control-Allow-Origin, сбоя запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="29966-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="29966-188">В частности браузер запрещает запрос.</span><span class="sxs-lookup"><span data-stu-id="29966-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="29966-189">Даже если сервер возвращает успешный ответ, браузер не предоставить ответ клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="29966-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="29966-190">**Предварительные запросы**</span><span class="sxs-lookup"><span data-stu-id="29966-190">**Preflight Requests**</span></span>

<span data-ttu-id="29966-191">Для некоторых запросов CORS браузер посылает дополнительный запрос, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса.</span><span class="sxs-lookup"><span data-stu-id="29966-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="29966-192">Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="29966-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="29966-193">Метод запроса — GET, HEAD или POST, *и*</span><span class="sxs-lookup"><span data-stu-id="29966-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="29966-194">Приложение не устанавливает все заголовки запроса, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, *и*</span><span class="sxs-lookup"><span data-stu-id="29966-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="29966-195">Заголовок Content-Type (если задать) является одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="29966-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="29966-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="29966-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="29966-197">данные multipart/формы</span><span class="sxs-lookup"><span data-stu-id="29966-197">multipart/form-data</span></span>
    - <span data-ttu-id="29966-198">text/plain</span><span class="sxs-lookup"><span data-stu-id="29966-198">text/plain</span></span>

<span data-ttu-id="29966-199">Правило о заголовках запроса применяется к заголовки, которые приложение задает путем вызова **setRequestHeader** на **XMLHttpRequest** объекта.</span><span class="sxs-lookup"><span data-stu-id="29966-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="29966-200">(Спецификации CORS вызывает эти «заголовки запроса автора»). Правило не применяется к заголовкам *браузера* можно задать, например User-Agent, узла или Content-Length.</span><span class="sxs-lookup"><span data-stu-id="29966-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="29966-201">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="29966-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="29966-202">Возможность предварительного запроса используется метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="29966-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="29966-203">Он включает два специальных заголовков:</span><span class="sxs-lookup"><span data-stu-id="29966-203">It includes two special headers:</span></span>

- <span data-ttu-id="29966-204">Access-Control-Request-Method: Метод HTTP, будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="29966-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="29966-205">Access-Control-Request-Headers: Список заголовков запросов, *приложения* задать для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="29966-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="29966-206">(Опять же, это не включает заголовки, которые задает браузера.)</span><span class="sxs-lookup"><span data-stu-id="29966-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="29966-207">Ниже приведен пример ответа, при условии, что сервер разрешает запрос.</span><span class="sxs-lookup"><span data-stu-id="29966-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="29966-208">Ответ содержит заголовок Access-Control-Allow-Methods, в которой перечислены разрешенные методы и при необходимости заголовок Access-Control-разрешить-Headers, в которой перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="29966-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="29966-209">Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="29966-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="29966-210">Правила области видимости для [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="29966-210">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="29966-211">Вы можете включить CORS каждого действия, отдельного контроллера или глобально для всех контроллеров веб-API в приложении.</span><span class="sxs-lookup"><span data-stu-id="29966-211">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="29966-212">**Каждого действия**</span><span class="sxs-lookup"><span data-stu-id="29966-212">**Per Action**</span></span>

<span data-ttu-id="29966-213">Чтобы включить CORS для одного действия, задайте **[EnableCors]** атрибут в методе действия.</span><span class="sxs-lookup"><span data-stu-id="29966-213">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="29966-214">В следующем примере включается CORS для `GetItem` только метод.</span><span class="sxs-lookup"><span data-stu-id="29966-214">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="29966-215">**Для контроллера**</span><span class="sxs-lookup"><span data-stu-id="29966-215">**Per Controller**</span></span>

<span data-ttu-id="29966-216">Если задать **[EnableCors]** класса контроллера, он применяется ко всем действиям в контроллере.</span><span class="sxs-lookup"><span data-stu-id="29966-216">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="29966-217">Чтобы отключить CORS для действия, добавьте **[DisableCors]** атрибут к действию.</span><span class="sxs-lookup"><span data-stu-id="29966-217">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="29966-218">В следующем примере включается CORS для каждого метода, за исключением `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="29966-218">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="29966-219">**Глобально**</span><span class="sxs-lookup"><span data-stu-id="29966-219">**Globally**</span></span>

<span data-ttu-id="29966-220">Чтобы включить CORS для всех контроллеров веб-API в приложении, передайте **EnableCorsAttribute** экземпляр **EnableCors** метод:</span><span class="sxs-lookup"><span data-stu-id="29966-220">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="29966-221">Если задать атрибут на более чем одну область, является порядок приоритета:</span><span class="sxs-lookup"><span data-stu-id="29966-221">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="29966-222">Действие</span><span class="sxs-lookup"><span data-stu-id="29966-222">Action</span></span>
2. <span data-ttu-id="29966-223">Контроллер</span><span class="sxs-lookup"><span data-stu-id="29966-223">Controller</span></span>
3. <span data-ttu-id="29966-224">Global</span><span class="sxs-lookup"><span data-stu-id="29966-224">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="29966-225">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="29966-225">Set the allowed origins</span></span>

<span data-ttu-id="29966-226">*Источников* параметр **[EnableCors]** атрибут указывает, какие источники разрешены для доступа к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="29966-226">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="29966-227">Значение — разделенный запятыми список разрешенных источников.</span><span class="sxs-lookup"><span data-stu-id="29966-227">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="29966-228">Можно также использовать подстановочное значение "\*" разрешать запросы от любого источников.</span><span class="sxs-lookup"><span data-stu-id="29966-228">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="29966-229">Тщательно обдумайте прежде чем разрешить запросы из любого источника.</span><span class="sxs-lookup"><span data-stu-id="29966-229">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="29966-230">Это означает, что буквально любой веб-сайт может выполнять вызовы AJAX в веб-API.</span><span class="sxs-lookup"><span data-stu-id="29966-230">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="29966-231">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="29966-231">Set the allowed HTTP methods</span></span>

<span data-ttu-id="29966-232">*Методы* параметр **[EnableCors]** атрибут указывает, какие методы HTTP получают доступ к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="29966-232">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="29966-233">Чтобы разрешить все методы, используйте подстановочное значение "\*«.</span><span class="sxs-lookup"><span data-stu-id="29966-233">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="29966-234">Следующий пример разрешает только запросы GET и POST.</span><span class="sxs-lookup"><span data-stu-id="29966-234">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="29966-235">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="29966-235">Set the allowed request headers</span></span>

<span data-ttu-id="29966-236">В этой статье описано, как ранее Предварительный запрос может включать заголовок Access-Control-Request-Headers, список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).</span><span class="sxs-lookup"><span data-stu-id="29966-236">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="29966-237">*Заголовки* параметр **[EnableCors]** атрибут указывает, какие заголовки запроса автор разрешены.</span><span class="sxs-lookup"><span data-stu-id="29966-237">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="29966-238">Чтобы разрешить любые заголовки, задайте *заголовки* для "\*«.</span><span class="sxs-lookup"><span data-stu-id="29966-238">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="29966-239">Чтобы добавить в список разрешений определенные заголовки, задайте *заголовки* в список с разделителями запятыми в разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="29966-239">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="29966-240">Однако браузеры не согласованы полностью в установке Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="29966-240">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="29966-241">Например Chrome в настоящее время содержит «origin».</span><span class="sxs-lookup"><span data-stu-id="29966-241">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="29966-242">FireFox не включает стандартные заголовки, такие как «Принять», даже в том случае, когда приложение устанавливает их в скрипте.</span><span class="sxs-lookup"><span data-stu-id="29966-242">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="29966-243">Если задать *заголовки* на что-либо, отличное от «\*», следует включать по крайней мере «принять,» «content-type» и «началом координат», а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="29966-243">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="29966-244">Набор допустимых заголовков</span><span class="sxs-lookup"><span data-stu-id="29966-244">Set the allowed response headers</span></span>

<span data-ttu-id="29966-245">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="29966-245">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="29966-246">Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="29966-246">The response headers that are available by default are:</span></span>

- <span data-ttu-id="29966-247">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="29966-247">Cache-Control</span></span>
- <span data-ttu-id="29966-248">Content-Language</span><span class="sxs-lookup"><span data-stu-id="29966-248">Content-Language</span></span>
- <span data-ttu-id="29966-249">Content-Type</span><span class="sxs-lookup"><span data-stu-id="29966-249">Content-Type</span></span>
- <span data-ttu-id="29966-250">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="29966-250">Expires</span></span>
- <span data-ttu-id="29966-251">Дата последнего изменения</span><span class="sxs-lookup"><span data-stu-id="29966-251">Last-Modified</span></span>
- <span data-ttu-id="29966-252">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="29966-252">Pragma</span></span>

<span data-ttu-id="29966-253">Спецификация CORS вызывает эти [заголовки ответа на простой](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="29966-253">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="29966-254">Чтобы сделать доступными для приложения другие заголовки, установите *exposedHeaders* параметр **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="29966-254">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="29966-255">В следующем примере, контроллер `Get` метод задает настраиваемый заголовок с именем «X-Custom-Header».</span><span class="sxs-lookup"><span data-stu-id="29966-255">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="29966-256">По умолчанию браузер не будет предоставлять этот заголовок в запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="29966-256">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="29966-257">Чтобы сделать доступным заголовок, включите «X-Custom-Header» в *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="29966-257">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="29966-258">Передать учетные данные запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="29966-258">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="29966-259">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="29966-259">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="29966-260">По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="29966-260">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="29966-261">Учетные данные содержат файлы cookie, а также схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="29966-261">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="29966-262">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать **XMLHttpRequest.withCredentials** значение true.</span><span class="sxs-lookup"><span data-stu-id="29966-262">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="29966-263">С помощью **XMLHttpRequest** напрямую:</span><span class="sxs-lookup"><span data-stu-id="29966-263">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="29966-264">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="29966-264">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="29966-265">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="29966-265">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="29966-266">Чтобы разрешить учетные данные от источника в веб-API, задайте **SupportsCredentials** присвоено значение true, если **[EnableCors]** атрибут:</span><span class="sxs-lookup"><span data-stu-id="29966-266">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="29966-267">Если это свойство имеет значение true, HTTP-ответа будет включать заголовок доступа-элемент управления-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="29966-267">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="29966-268">Этот заголовок указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.</span><span class="sxs-lookup"><span data-stu-id="29966-268">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="29966-269">Если браузер отправляет учетные данные, но ответ не включает допустимый заголовка Access-элемент управления-Allow-Credentials, браузер не будет предоставлять доступ приложению ответ и происходит сбой запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="29966-269">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="29966-270">Соблюдайте осторожность при параметр **SupportsCredentials** значение true, так как это означает, что веб-сайт в другом домене может передать учетные данные вошедшего в систему пользователя веб-API от имени пользователя без оповещения пользователя.</span><span class="sxs-lookup"><span data-stu-id="29966-270">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="29966-271">Спецификации CORS также гласит, что параметр *источников* для &quot; \* &quot; является недопустимым при **SupportsCredentials** имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="29966-271">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="29966-272">Настраиваемые поставщики политики CORS</span><span class="sxs-lookup"><span data-stu-id="29966-272">Custom CORS policy providers</span></span>

<span data-ttu-id="29966-273">**[EnableCors]** атрибут реализует **ICorsPolicyProvider** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="29966-273">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="29966-274">Можно предоставить собственную реализацию, создав класс, производный от **атрибут** и реализует **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="29966-274">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="29966-275">Теперь можно применить атрибут, в любом месте, что можно поместить **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="29966-275">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="29966-276">Например пользовательский поставщик политики CORS удалось считать параметры из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="29966-276">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="29966-277">В качестве альтернативы с помощью атрибутов, вы можете зарегистрировать **ICorsPolicyProviderFactory** объект, который создает **ICorsPolicyProvider** объектов.</span><span class="sxs-lookup"><span data-stu-id="29966-277">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="29966-278">Чтобы задать **ICorsPolicyProviderFactory**, вызовите **SetCorsPolicyProviderFactory** метод расширения во время запуска, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="29966-278">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="29966-279">Поддержка браузеров</span><span class="sxs-lookup"><span data-stu-id="29966-279">Browser support</span></span>

<span data-ttu-id="29966-280">Пакет CORS веб-API — это технология на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="29966-280">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="29966-281">Браузер пользователя также должен поддерживать CORS.</span><span class="sxs-lookup"><span data-stu-id="29966-281">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="29966-282">К счастью, в текущих версиях все основные браузеры включают [поддержка CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="29966-282">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
