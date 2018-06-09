---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Включение запросов независимо от источника в ASP.NET Web API 2 | Документы Microsoft
author: MikeWasson
description: Показано, как поддержка общего доступа к ресурсам независимо от источника (CORS) в веб-API ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508383"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="81639-103">Включение запросов независимо от источника в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="81639-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="81639-104">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="81639-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="81639-105">Безопасность обозревателя предотвращает внесение запросы AJAX в другой домен на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="81639-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="81639-106">Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="81639-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="81639-107">Однако иногда может потребоваться разрешить другие сайты вызвать ваш веб-API.</span><span class="sxs-lookup"><span data-stu-id="81639-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="81639-108">[Кросс-общий доступ к ресурсам источника](http://www.w3.org/TR/cors/) (CORS) — это стандарт W3C, которая позволяет серверу ослабить политика одного источника.</span><span class="sxs-lookup"><span data-stu-id="81639-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="81639-109">С помощью CORS, сервер можно явно разрешить некоторые запросы независимо от источника при отклонении другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="81639-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="81639-110">CORS является более безопасным и более гибким, чем ранее методов, например [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="81639-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="81639-111">Этого учебника показано, как включить CORS в приложении веб-API.</span><span class="sxs-lookup"><span data-stu-id="81639-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="81639-112">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="81639-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="81639-113">Visual Studio 2013 с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="81639-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="81639-114">Веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="81639-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="81639-115">Вступление</span><span class="sxs-lookup"><span data-stu-id="81639-115">Introduction</span></span>

<span data-ttu-id="81639-116">Этот учебник демонстрирует поддержку CORS в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81639-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="81639-117">Мы начнем с создания двух проектов ASP.NET — один вызываемой» веб-служба», где размещается контроллер веб-API, и другие вызываемой «WebClient», который вызывает веб-службы.</span><span class="sxs-lookup"><span data-stu-id="81639-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="81639-118">Так как два приложения размещены в разных доменах, AJAX-запросом из веб-клиента для веб-службы — это запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="81639-119">Что такое «Того же происхождения»?</span><span class="sxs-lookup"><span data-stu-id="81639-119">What is "Same Origin"?</span></span>

<span data-ttu-id="81639-120">Два URL-адреса имеют того же источника, если они имеют одинаковые схемы, узлов и портов.</span><span class="sxs-lookup"><span data-stu-id="81639-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="81639-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="81639-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="81639-122">Эти два URL-адреса имеют того же источника:</span><span class="sxs-lookup"><span data-stu-id="81639-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="81639-123">Эти URL-адреса имеют различные источники, чем предыдущий два:</span><span class="sxs-lookup"><span data-stu-id="81639-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="81639-124">`http://example.net` -Другой домен</span><span class="sxs-lookup"><span data-stu-id="81639-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="81639-125">`http://example.com:9000/foo.html` -Другой порт</span><span class="sxs-lookup"><span data-stu-id="81639-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="81639-126">`https://example.com/foo.html` -Другой схемы</span><span class="sxs-lookup"><span data-stu-id="81639-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="81639-127">`http://www.example.com/foo.html` -Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="81639-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="81639-128">При сравнении источников Internet Explorer не учитывает порт.</span><span class="sxs-lookup"><span data-stu-id="81639-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="81639-129">Создание проекта веб-службы</span><span class="sxs-lookup"><span data-stu-id="81639-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="81639-130">В этом разделе предполагается, что вы уже умеете создавать проекты веб-API.</span><span class="sxs-lookup"><span data-stu-id="81639-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="81639-131">См. в противном случае [Приступая к работе с веб-API ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="81639-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="81639-132">Запустите Visual Studio и создайте новый **веб-приложение ASP.NET** проекта.</span><span class="sxs-lookup"><span data-stu-id="81639-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="81639-133">Выберите **пустой** шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="81639-133">Select the **Empty** project template.</span></span> <span data-ttu-id="81639-134">В разделе «Добавление папок и основные ссылки для» выберите **веб-API** флажок.</span><span class="sxs-lookup"><span data-stu-id="81639-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="81639-135">При необходимости выберите параметр «Разместить в облаке», чтобы развернуть приложение в Mircosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="81639-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="81639-136">Корпорация Майкрософт предлагает бесплатные услуг хостинга до 10 веб-сайтов в [освободить пробной учетной записи Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="81639-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="81639-137">Добавить контроллер веб-API с именем `TestController` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="81639-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="81639-138">Можно запустить приложение локально или развернуть в Azure.</span><span class="sxs-lookup"><span data-stu-id="81639-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="81639-139">(Снимки экрана, в этом учебнике, я развертывается в веб-приложениях службы приложений Azure.) Чтобы проверить работоспособность веб-API, перейдите в папку `http://hostname/api/test/`, где *hostname* — это домен, в котором приложение было развернуто.</span><span class="sxs-lookup"><span data-stu-id="81639-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="81639-140">Вы должны увидеть текст ответа, &quot;получить: тестовое сообщение&quot;.</span><span class="sxs-lookup"><span data-stu-id="81639-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="81639-141">Создание проекта веб-клиента</span><span class="sxs-lookup"><span data-stu-id="81639-141">Create the WebClient Project</span></span>

<span data-ttu-id="81639-142">Создайте другой проект веб-приложения ASP.NET и выберите **MVC** шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="81639-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="81639-143">При необходимости установите **изменить аутентификацию** > **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="81639-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="81639-144">Не требуется проверка подлинности для этого учебника.</span><span class="sxs-lookup"><span data-stu-id="81639-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="81639-145">В обозревателе решений откройте файл Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="81639-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="81639-146">Замените код в этот файл следующее:</span><span class="sxs-lookup"><span data-stu-id="81639-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="81639-147">Для *serviceUrl* переменной, используйте URI-адрес веб-службы приложения.</span><span class="sxs-lookup"><span data-stu-id="81639-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="81639-148">Сейчас WebClient приложение запускается локально или опубликовать его в другой веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="81639-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="81639-149">Нажав кнопку «Попробовать» отправляет запрос AJAX в приложение веб-службы с помощью метода HTTP, перечисленные в раскрывающемся списке (GET, POST или PUT).</span><span class="sxs-lookup"><span data-stu-id="81639-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="81639-150">Это позволяет проверять различные запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="81639-151">В данный момент, приложения веб-службы не поддерживают CORS, поэтому при нажатии кнопки, возникнет ошибка.</span><span class="sxs-lookup"><span data-stu-id="81639-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="81639-152">Если посмотреть HTTP-трафика в средстве, например [Fiddler](http://www.telerik.com/fiddler), вы увидите, что браузер отправляет запрос GET и запрос выполнен успешно, но вызов AJAX возвращает ошибку.</span><span class="sxs-lookup"><span data-stu-id="81639-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="81639-153">Важно понимать, что политика одного источника не запрещает браузера из *отправки* запроса.</span><span class="sxs-lookup"><span data-stu-id="81639-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="81639-154">Вместо этого он предотвращает приложения просматривать *ответ*.</span><span class="sxs-lookup"><span data-stu-id="81639-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="81639-155">Включить CORS</span><span class="sxs-lookup"><span data-stu-id="81639-155">Enable CORS</span></span>

<span data-ttu-id="81639-156">Теперь давайте включить CORS в приложении веб-службы.</span><span class="sxs-lookup"><span data-stu-id="81639-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="81639-157">Во-первых добавьте пакет CORS NuGet.</span><span class="sxs-lookup"><span data-stu-id="81639-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="81639-158">В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="81639-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="81639-159">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="81639-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="81639-160">Эта команда устанавливает последнюю версию пакета и обновляет все зависимости, в том числе основных библиотек веб-API.</span><span class="sxs-lookup"><span data-stu-id="81639-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="81639-161">Пользователь - флаг версии для конкретной версии.</span><span class="sxs-lookup"><span data-stu-id="81639-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="81639-162">CORS пакету требуется веб-API 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="81639-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="81639-163">Откройте файл приложения\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="81639-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="81639-164">Добавьте следующий код в **WebApiConfig.Register** метод.</span><span class="sxs-lookup"><span data-stu-id="81639-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="81639-165">Добавьте **[EnableCors]** атрибут `TestController` класса:</span><span class="sxs-lookup"><span data-stu-id="81639-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="81639-166">Для *источников* параметра, используйте URI, где WebClient приложение было развернуто.</span><span class="sxs-lookup"><span data-stu-id="81639-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="81639-167">Это позволит запросы независимо от источника из веб-клиента, при этом по-прежнему Запрет всех других запросов между доменами.</span><span class="sxs-lookup"><span data-stu-id="81639-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="81639-168">Позднее, будут описаны параметры для **[EnableCors]** более подробно.</span><span class="sxs-lookup"><span data-stu-id="81639-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="81639-169">Не содержать косую черту в конце *источников* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="81639-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="81639-170">Повторно разверните обновленное приложение веб-службы.</span><span class="sxs-lookup"><span data-stu-id="81639-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="81639-171">Обновить веб-клиента не требуется.</span><span class="sxs-lookup"><span data-stu-id="81639-171">You don't need to update WebClient.</span></span> <span data-ttu-id="81639-172">Теперь запрос AJAX из веб-клиента должны выполняться успешно.</span><span class="sxs-lookup"><span data-stu-id="81639-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="81639-173">Методы GET, PUT и POST все разрешены.</span><span class="sxs-lookup"><span data-stu-id="81639-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="81639-174">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="81639-174">How CORS Works</span></span>

<span data-ttu-id="81639-175">В этом разделе описывается, что происходит в запрос CORS, на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="81639-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="81639-176">Очень важно понять, как работает CORS, и можно настроить **[EnableCors]** атрибут правильно и устранения неполадок, если не все работает должным образом.</span><span class="sxs-lookup"><span data-stu-id="81639-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="81639-177">Спецификация CORS представлены несколько заголовки HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="81639-178">Если браузер поддерживает CORS, он устанавливает эти заголовки автоматически для запросов независимо от источника. не требуется никаких действий в коде JavaScript.</span><span class="sxs-lookup"><span data-stu-id="81639-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="81639-179">Ниже приведен пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="81639-180">Заголовок «Источник» предоставляет домену узла, который выполняет запрос.</span><span class="sxs-lookup"><span data-stu-id="81639-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="81639-181">Если сервер разрешает запрос, он устанавливает заголовка Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="81639-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="81639-182">Значение этого заголовка соответствует заголовку источника либо является использование подстановочного знака «\*», это значит, что разрешены любые источники.</span><span class="sxs-lookup"><span data-stu-id="81639-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="81639-183">Если ответ содержит Access-Control-Allow-Origin заголовок, происходит сбой запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="81639-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="81639-184">В частности браузер блокирует запрос.</span><span class="sxs-lookup"><span data-stu-id="81639-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="81639-185">Даже если сервер возвращает успешный ответ, браузер не делает ответ доступной клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="81639-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="81639-186">**Предварительные запросы**</span><span class="sxs-lookup"><span data-stu-id="81639-186">**Preflight Requests**</span></span>

<span data-ttu-id="81639-187">Для некоторых запросов CORS браузер отправляет запрос на дополнительные, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса.</span><span class="sxs-lookup"><span data-stu-id="81639-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="81639-188">Браузер может пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="81639-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="81639-189">Метод запроса является GET, HEAD или POST, *и*</span><span class="sxs-lookup"><span data-stu-id="81639-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="81639-190">Приложение не устанавливает все заголовки запросов, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, *и*</span><span class="sxs-lookup"><span data-stu-id="81639-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="81639-191">Заголовок Content-Type (если задать) является одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="81639-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="81639-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="81639-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="81639-193">данные multipart/формы</span><span class="sxs-lookup"><span data-stu-id="81639-193">multipart/form-data</span></span>
    - <span data-ttu-id="81639-194">text/plain.</span><span class="sxs-lookup"><span data-stu-id="81639-194">text/plain</span></span>

<span data-ttu-id="81639-195">Правило о заголовках запроса применяется к заголовки, которые приложение задает путем вызова **setRequestHeader** на **XMLHttpRequest** объекта.</span><span class="sxs-lookup"><span data-stu-id="81639-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="81639-196">(Спецификации CORS вызывает эти «автор запроса заголовки»). Правило не применяется к заголовкам *браузера* можно задать, например User-Agent, узлу или Content-Length.</span><span class="sxs-lookup"><span data-stu-id="81639-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="81639-197">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="81639-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="81639-198">Предварительный запрос с помощью метода HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="81639-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="81639-199">Он включает два особых заголовков:</span><span class="sxs-lookup"><span data-stu-id="81639-199">It includes two special headers:</span></span>

- <span data-ttu-id="81639-200">Access-Control-Request-Method: HTTP метод, который будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="81639-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="81639-201">Access-Control-Request-Headers: Список заголовков запроса, *приложения* задать для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="81639-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="81639-202">(Опять же, это не включает заголовки, которые задает браузера.)</span><span class="sxs-lookup"><span data-stu-id="81639-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="81639-203">Ниже приведен пример ответа, при условии, что сервер разрешает запрос:</span><span class="sxs-lookup"><span data-stu-id="81639-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="81639-204">Ответ включает и-методы управления доступом — разрешить заголовок, который содержит список допустимых методов и при необходимости заголовок Access-Control-разрешить-Headers, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="81639-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="81639-205">Если Предварительный запрос завершается успешно, браузер отправляет сам запрос, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="81639-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="81639-206">Правила областей для [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="81639-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="81639-207">Можно включить CORS каждого действия каждого контроллера или глобально для всех контроллеров веб-API в приложении.</span><span class="sxs-lookup"><span data-stu-id="81639-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="81639-208">**Каждого действия**</span><span class="sxs-lookup"><span data-stu-id="81639-208">**Per Action**</span></span>

<span data-ttu-id="81639-209">Чтобы включить CORS для одного действия, установите **[EnableCors]** атрибута в методе действия.</span><span class="sxs-lookup"><span data-stu-id="81639-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="81639-210">В следующем примере включается CORS для `GetItem` только метод.</span><span class="sxs-lookup"><span data-stu-id="81639-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="81639-211">**На каждый контроллер**</span><span class="sxs-lookup"><span data-stu-id="81639-211">**Per Controller**</span></span>

<span data-ttu-id="81639-212">Если задать **[EnableCors]** класса контроллера, оно применяется ко всем действиям в контроллере.</span><span class="sxs-lookup"><span data-stu-id="81639-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="81639-213">Чтобы отключить CORS для действия, добавьте **[DisableCors]** атрибут действия.</span><span class="sxs-lookup"><span data-stu-id="81639-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="81639-214">В следующем примере включается CORS для каждого метода, за исключением `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="81639-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="81639-215">**Глобально**</span><span class="sxs-lookup"><span data-stu-id="81639-215">**Globally**</span></span>

<span data-ttu-id="81639-216">Чтобы включить CORS для всех контроллеров веб-API в приложении, необходимо передать **EnableCorsAttribute** экземпляр **EnableCors** метод:</span><span class="sxs-lookup"><span data-stu-id="81639-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="81639-217">Если задать атрибут на более чем одну область, является порядок приоритета:</span><span class="sxs-lookup"><span data-stu-id="81639-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="81639-218">Действие</span><span class="sxs-lookup"><span data-stu-id="81639-218">Action</span></span>
2. <span data-ttu-id="81639-219">Контроллер</span><span class="sxs-lookup"><span data-stu-id="81639-219">Controller</span></span>
3. <span data-ttu-id="81639-220">Global</span><span class="sxs-lookup"><span data-stu-id="81639-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="81639-221">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="81639-221">Set the Allowed Origins</span></span>

<span data-ttu-id="81639-222">*Источников* параметр **[EnableCors]** атрибут указывает, какие источники, которые получают доступ к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="81639-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="81639-223">Значение — список разрешенных источников с разделителями запятыми.</span><span class="sxs-lookup"><span data-stu-id="81639-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="81639-224">Можно также использовать использование подстановочного знака "\*" разрешать запросы от любого источников.</span><span class="sxs-lookup"><span data-stu-id="81639-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="81639-225">Внимательно рассмотрите перед предоставлением запросов из любого источника.</span><span class="sxs-lookup"><span data-stu-id="81639-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="81639-226">Он означает, что практически любой веб-сайта можно вносить вызовы AJAX для веб-API.</span><span class="sxs-lookup"><span data-stu-id="81639-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="81639-227">Набор разрешенных HTTP-методы</span><span class="sxs-lookup"><span data-stu-id="81639-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="81639-228">*Методы* параметр **[EnableCors]** атрибут задает HTTP-методы, получают доступ к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="81639-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="81639-229">Чтобы разрешить все методы, используйте подстановочный знак значение «\*».</span><span class="sxs-lookup"><span data-stu-id="81639-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="81639-230">В следующем примере пользователь запросов GET и POST.</span><span class="sxs-lookup"><span data-stu-id="81639-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="81639-231">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="81639-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="81639-232">Ранее я описал как Предварительный запрос может включать заголовок Access-Control-Request-Headers список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).</span><span class="sxs-lookup"><span data-stu-id="81639-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="81639-233">*Заголовки* параметр **[EnableCors]** атрибут указывает, какие заголовки запроса автор разрешены.</span><span class="sxs-lookup"><span data-stu-id="81639-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="81639-234">Чтобы разрешить все заголовки, задайте *заголовки* для «\*».</span><span class="sxs-lookup"><span data-stu-id="81639-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="81639-235">Чтобы белый список специальные заголовки, установите *заголовки* в список с разделителями запятыми допустимых заголовков:</span><span class="sxs-lookup"><span data-stu-id="81639-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="81639-236">Однако браузеры не полностью соответствуют в установке Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="81639-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="81639-237">Например в настоящее время включает Chrome «источник»; Хотя FireFox не включает стандартные заголовки, такие как «Accept», даже в том случае, когда приложение устанавливает их в скрипт.</span><span class="sxs-lookup"><span data-stu-id="81639-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="81639-238">Если задать *заголовки* на что-либо, отличное от «\*», следует включать по крайней мере «принять,» «content-type» и «источник», а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="81639-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="81639-239">Задать заголовки ответа разрешенных</span><span class="sxs-lookup"><span data-stu-id="81639-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="81639-240">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="81639-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="81639-241">Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="81639-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="81639-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="81639-242">Cache-Control</span></span>
- <span data-ttu-id="81639-243">Content-Language</span><span class="sxs-lookup"><span data-stu-id="81639-243">Content-Language</span></span>
- <span data-ttu-id="81639-244">Тип содержимого</span><span class="sxs-lookup"><span data-stu-id="81639-244">Content-Type</span></span>
- <span data-ttu-id="81639-245">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="81639-245">Expires</span></span>
- <span data-ttu-id="81639-246">Дата последнего изменения</span><span class="sxs-lookup"><span data-stu-id="81639-246">Last-Modified</span></span>
- <span data-ttu-id="81639-247">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="81639-247">Pragma</span></span>

<span data-ttu-id="81639-248">Спецификация CORS вызывает эти [заголовки ответа на простой](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="81639-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="81639-249">Чтобы настроить другие заголовки, доступны для приложения, установить *exposedHeaders* параметр **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="81639-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="81639-250">В следующем примере, контроллер `Get` метод задает пользовательский заголовок с именем «X пользовательский заголовок».</span><span class="sxs-lookup"><span data-stu-id="81639-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="81639-251">По умолчанию браузер не будет предоставлять этот заголовок в запросе независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="81639-252">Чтобы сделать доступным заголовок, включают «X пользовательский заголовок» в *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="81639-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="81639-253">Передача учетных данных в запросы независимо от источника</span><span class="sxs-lookup"><span data-stu-id="81639-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="81639-254">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="81639-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="81639-255">По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="81639-256">Учетные данные включают файлы cookie, а также схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="81639-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="81639-257">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать **XMLHttpRequest.withCredentials** значение true.</span><span class="sxs-lookup"><span data-stu-id="81639-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="81639-258">С помощью **XMLHttpRequest** напрямую:</span><span class="sxs-lookup"><span data-stu-id="81639-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="81639-259">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="81639-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="81639-260">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="81639-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="81639-261">Чтобы разрешить независимо от источника учетные данные в веб-API, задайте **SupportsCredentials** свойство значение true в **[EnableCors]** атрибута:</span><span class="sxs-lookup"><span data-stu-id="81639-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="81639-262">Если это свойство имеет значение true, HTTP-ответ будет содержать заголовок доступа-элемент управления-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="81639-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="81639-263">Этот заголовок предписывает браузеру, поддерживает ли сервер учетные данные для запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="81639-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="81639-264">Если браузер отправляет учетные данные, но ответа не содержит допустимый заголовок доступа-элемент управления-Allow-Credentials, браузер не предоставляет приложению ответ и происходит сбой запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="81639-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="81639-265">Должны быть очень осторожными параметр **SupportsCredentials** значение true, так как это означает, что веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя веб-API от имени пользователя без оповещения пользователя.</span><span class="sxs-lookup"><span data-stu-id="81639-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="81639-266">Спецификация CORS указывается этот параметр *источников* для &quot; \* &quot; недопустим при **SupportsCredentials** имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="81639-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="81639-267">Поставщики пользовательских CORS политики</span><span class="sxs-lookup"><span data-stu-id="81639-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="81639-268">**[EnableCors]** атрибут реализует **ICorsPolicyProvider** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="81639-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="81639-269">Можно предоставить собственную реализацию, создав класс, производный от **атрибута** и реализует **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="81639-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="81639-270">Теперь можно применить атрибут, в любом месте, что нужно будет только разместить **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="81639-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="81639-271">Например пользовательский поставщик политики CORS удалось считать параметры из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="81639-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="81639-272">В качестве альтернативы, используя атрибуты, можно зарегистрировать **ICorsPolicyProviderFactory** объект, который создает **ICorsPolicyProvider** объектов.</span><span class="sxs-lookup"><span data-stu-id="81639-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="81639-273">Чтобы задать **ICorsPolicyProviderFactory**, вызовите **SetCorsPolicyProviderFactory** метод расширения во время запуска, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="81639-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="81639-274">Поддержка браузеров</span><span class="sxs-lookup"><span data-stu-id="81639-274">Browser Support</span></span>

<span data-ttu-id="81639-275">Пакет Web API CORS не серверные технологии.</span><span class="sxs-lookup"><span data-stu-id="81639-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="81639-276">В браузере пользователя необходимо также поддерживают CORS.</span><span class="sxs-lookup"><span data-stu-id="81639-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="81639-277">К счастью, все основные обозреватели текущие версии включают [поддержка CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="81639-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="81639-278">Internet Explorer 8 и Internet Explorer 9 имеют частичная поддержка CORS, вместо прежних версий XDomainRequest, который обеспечивает объект XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="81639-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="81639-279">Дополнительные сведения см. в разделе [XDomainRequest, который обеспечивает - ограничения, ограничения и способы решения проблем](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="81639-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
