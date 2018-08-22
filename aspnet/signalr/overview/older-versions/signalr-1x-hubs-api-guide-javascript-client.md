---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x по API концентраторов — клиента JavaScript | Документация Майкрософт
author: pfletcher
description: Этот документ представляет собой введение по API концентраторов SignalR версии 1.1 в клиентах JavaScript, таких как браузеры и Store Windows (WinJS) рабоче...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 993ad7924d8335f79aa2c3e41c00ddfa8bc26874
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828457"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="2f646-103">SignalR 1.x по API концентраторов — клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f646-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="2f646-104">по [Флетчера Патрик](https://github.com/pfletcher), [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2f646-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="2f646-105">Этот документ содержит вводные сведения по API концентраторов SignalR версии 1.1 в клиентах JavaScript, таких как браузеры и приложения Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="2f646-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="2f646-106">API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу.</span><span class="sxs-lookup"><span data-stu-id="2f646-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="2f646-107">В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="2f646-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="2f646-108">В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере.</span><span class="sxs-lookup"><span data-stu-id="2f646-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="2f646-109">SignalR берет на себя все необходимое для вас клиент сервер.</span><span class="sxs-lookup"><span data-stu-id="2f646-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="2f646-110">SignalR также предлагает API низкого уровня, вызывается постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="2f646-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="2f646-111">Введение в SignalR, концентраторы и постоянные подключения, или в этом учебнике показано, как создать полное приложение SignalR, см. в разделе [SignalR — Приступая к работе](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f646-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="2f646-112">Обзор</span><span class="sxs-lookup"><span data-stu-id="2f646-112">Overview</span></span>

<span data-ttu-id="2f646-113">Этот документ содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="2f646-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="2f646-114">Созданный прокси и что он делает для вас</span><span class="sxs-lookup"><span data-stu-id="2f646-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="2f646-115">Когда следует использовать созданный прокси</span><span class="sxs-lookup"><span data-stu-id="2f646-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="2f646-116">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="2f646-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="2f646-117">Способ создания ссылок динамически создаваемого прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="2f646-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="2f646-118">Как создать физический файл для SignalR созданный прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="2f646-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="2f646-119">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="2f646-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="2f646-120">$. connection.hub совпадает с объектом этого $.hubConnection() создает</span><span class="sxs-lookup"><span data-stu-id="2f646-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="2f646-121">Асинхронное выполнение метода start</span><span class="sxs-lookup"><span data-stu-id="2f646-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="2f646-122">Как установить подключение между доменами</span><span class="sxs-lookup"><span data-stu-id="2f646-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="2f646-123">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="2f646-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="2f646-124">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="2f646-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="2f646-125">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="2f646-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="2f646-126">Как получить учетную запись-посредник для класса концентратора</span><span class="sxs-lookup"><span data-stu-id="2f646-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="2f646-127">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="2f646-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="2f646-128">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="2f646-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="2f646-129">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="2f646-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="2f646-130">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="2f646-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="2f646-131">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="2f646-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="2f646-132">Документацию по программированию сервера или клиентов .NET, ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="2f646-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="2f646-133">Руководство по API концентраторов SignalR - сервер</span><span class="sxs-lookup"><span data-stu-id="2f646-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="2f646-134">Руководство по API концентраторов SignalR — клиент .NET</span><span class="sxs-lookup"><span data-stu-id="2f646-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="2f646-135">Приведены ссылки на разделы, справочник по API .NET 4.5 версия API.</span><span class="sxs-lookup"><span data-stu-id="2f646-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="2f646-136">Если вы используете .NET 4, см. в разделе [версия .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="2f646-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="2f646-137">Созданный прокси и что он делает для вас</span><span class="sxs-lookup"><span data-stu-id="2f646-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="2f646-138">Можно запрограммировать клиента JavaScript для взаимодействия со службой SignalR с или без прокси-сервер, созданный SignalR для вас.</span><span class="sxs-lookup"><span data-stu-id="2f646-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="2f646-139">Прокси-сервер может сделать для вас является упростить синтаксис кода можно использовать для подключения, методы записи, которые сервер вызывает метод, и вызывать методы на сервере.</span><span class="sxs-lookup"><span data-stu-id="2f646-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="2f646-140">При написании кода для вызова методов сервера, созданного прокси позволяет использовать синтаксис, который выглядит так, будто выполнялись локальной функции: можно написать `serverMethod(arg1, arg2)` вместо `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="2f646-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="2f646-141">Если вы введете имя метода сервера синтаксис созданного прокси позволяет ошибку на стороне клиента будут понятны и окно интерпретации.</span><span class="sxs-lookup"><span data-stu-id="2f646-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="2f646-142">И если вручную создать файл, который определяет прокси-серверы, можно также обеспечить поддержку IntelliSense для написания кода, который вызывает методы сервера.</span><span class="sxs-lookup"><span data-stu-id="2f646-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="2f646-143">Например предположим, что у вас есть следующий класс концентратора на сервере:</span><span class="sxs-lookup"><span data-stu-id="2f646-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="2f646-144">В следующих примерах кода показано, как выглядит код JavaScript для вызова `NewContosoChatMessage` метод на сервере, а также получать вызовы `addContosoChatMessageToPage` метод с сервера.</span><span class="sxs-lookup"><span data-stu-id="2f646-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="2f646-145">**С помощью созданного прокси**</span><span class="sxs-lookup"><span data-stu-id="2f646-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="2f646-146">**Без созданный прокси-сервера**</span><span class="sxs-lookup"><span data-stu-id="2f646-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="2f646-147">Когда следует использовать созданный прокси</span><span class="sxs-lookup"><span data-stu-id="2f646-147">When to use the generated proxy</span></span>

<span data-ttu-id="2f646-148">Если вы хотите зарегистрировать несколько обработчиков событий для метод клиента, сервер вызывает метод, нельзя использовать созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="2f646-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="2f646-149">В противном случае вы можете использовать созданный прокси или не зависимости от используемого языка программирования.</span><span class="sxs-lookup"><span data-stu-id="2f646-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="2f646-150">Если вы решили не использовать его, нет необходимости ссылаться на «/ концентраторов signalr» URL-адрес в `script` элемент в коде клиента.</span><span class="sxs-lookup"><span data-stu-id="2f646-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="2f646-151">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="2f646-151">Client setup</span></span>

<span data-ttu-id="2f646-152">Клиент JavaScript требуются ссылки на jQuery и JavaScript core SignalR.</span><span class="sxs-lookup"><span data-stu-id="2f646-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="2f646-153">1.6.4 или основных более поздних версий, например 1.7.2, 1.8.2 или 1.9.1, требуется версия jQuery.</span><span class="sxs-lookup"><span data-stu-id="2f646-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="2f646-154">Если вы решили использовать созданный прокси, необходимо также ссылку на прокси-сервера SignalR созданный файл JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f646-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="2f646-155">В следующем примере показано, как может выглядеть ссылки на странице HTML, который использует созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="2f646-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="2f646-156">Эти ссылки должны быть включены в следующем порядке: jQuery, SignalR core после этого и прокси-серверы SignalR Фамилия.</span><span class="sxs-lookup"><span data-stu-id="2f646-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="2f646-157">Способ создания ссылок динамически создаваемого прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="2f646-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="2f646-158">В предыдущем примере ссылка на прокси-сервер создается SignalR — динамически созданный код JavaScript, не к какому файлу.</span><span class="sxs-lookup"><span data-stu-id="2f646-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="2f646-159">SignalR создает код JavaScript для прокси-сервера в режиме реального времени и обслуживает его клиенту в ответ на URL-адрес «/ signalr/концентраторы».</span><span class="sxs-lookup"><span data-stu-id="2f646-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="2f646-160">Если вы указали другой базовый URL-адрес для подключений SignalR на сервере в вашей `MapHubs` , URL-адрес динамически создаваемого файла посредника является пользовательский URL-адрес с «/ концентраторы» добавленным к нему.</span><span class="sxs-lookup"><span data-stu-id="2f646-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="2f646-161">Для клиентов Windows 8 (Windows Store) JavaScript используйте физической посредника вместо того, динамически создаваемый.</span><span class="sxs-lookup"><span data-stu-id="2f646-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="2f646-162">Дополнительные сведения см. в разделе [прокси, созданного как создать физический файл для SignalR](#manualproxy) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="2f646-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="2f646-163">В представлении ASP.NET MVC 4 Razor используйте тильда для обращения к корневой папке приложения в файл справки прокси-сервера:</span><span class="sxs-lookup"><span data-stu-id="2f646-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="2f646-164">Дополнительные сведения об использовании SignalR в MVC 4, см. в разделе [начало работы с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="2f646-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="2f646-165">В представлении Razor в ASP.NET MVC 3, используйте `Url.Content` для прокси-сервера файл справки:</span><span class="sxs-lookup"><span data-stu-id="2f646-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="2f646-166">В приложении веб-форм ASP.NET использовать `ResolveClientUrl` для ваших прокси-серверов, ссылка на файл, или зарегистрировать его с помощью ScriptManager, с помощью приложения корневой путь (начинающийся с тильды):</span><span class="sxs-lookup"><span data-stu-id="2f646-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="2f646-167">Как правило используйте тот же метод для указания URL-адреса с «/ signalr/концентраторы», используемого для файлов CSS или JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f646-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="2f646-168">При указании URL-адрес без использования тильда в некоторых сценариях приложение будет работать правильно при тестирования в Visual Studio, используя IIS Express, но будут завершаться сообщение об ошибке 404, при развертывании на полноценный сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="2f646-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="2f646-169">Дополнительные сведения см. в разделе **разрешении ссылок на ресурсы корневого уровня** в [веб-серверов в Visual Studio для веб-проектов ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="2f646-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="2f646-170">При запуске веб-проекта в Visual Studio 2012 в режиме отладки, и если вы используете Internet Explorer как браузер, вы увидите файл прокси в **обозревателе решений** под **документы скриптов**, как показано на на рисунке.</span><span class="sxs-lookup"><span data-stu-id="2f646-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Файл созданного прокси JavaScript в обозревателе решений](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="2f646-172">Чтобы просмотреть содержимое файла, дважды щелкните **концентраторов**.</span><span class="sxs-lookup"><span data-stu-id="2f646-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="2f646-173">Если вы не используете Visual Studio 2012 и Internet Explorer, или если вы не в режиме отладки, содержимое файла можно получить, перейдя по адресу «/ signalR/концентраторы» URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="2f646-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="2f646-174">Например, если ваш сайт работает в `http://localhost:56699`, перейдите в меню `http://localhost:56699/SignalR/hubs` в браузере.</span><span class="sxs-lookup"><span data-stu-id="2f646-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="2f646-175">Как создать физический файл для SignalR созданный прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="2f646-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="2f646-176">В качестве альтернативы на динамически созданный прокси-сервер можно создать физический файл, содержащий код прокси-сервера и ссылку на этот файл.</span><span class="sxs-lookup"><span data-stu-id="2f646-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="2f646-177">Вы можете это сделать, для контроля над кэшированием или объединение поведение, или для получения IntelliSense при написании кода вызовы методов сервера.</span><span class="sxs-lookup"><span data-stu-id="2f646-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="2f646-178">Чтобы создать прокси-файл, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="2f646-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="2f646-179">Установка [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="2f646-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="2f646-180">Откройте командную строку и перейдите к *средства* папку, содержащую файл SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="2f646-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="2f646-181">В папку средств находится по следующему адресу:</span><span class="sxs-lookup"><span data-stu-id="2f646-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="2f646-182">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2f646-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="2f646-183">Путь к вашей *.dll* обычно *bin* папку в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="2f646-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="2f646-184">Эта команда создает файл с именем *server.js* в той же папке, что *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="2f646-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="2f646-185">Поместите *server.js* в соответствующую папку в проекте, переименуйте его в зависимости от приложения и добавьте ссылку на него вместо ссылки на «/ концентраторов signalr».</span><span class="sxs-lookup"><span data-stu-id="2f646-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="2f646-186">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="2f646-186">How to establish a connection</span></span>

<span data-ttu-id="2f646-187">Чтобы установить подключение, необходимо создать объект подключения, создать учетную запись-посредник и регистрировать обработчики событий для методов, которые могут вызываться с сервера.</span><span class="sxs-lookup"><span data-stu-id="2f646-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="2f646-188">При настройке прокси-сервера и обработчики событий, установить соединение, вызвав `start` метод.</span><span class="sxs-lookup"><span data-stu-id="2f646-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="2f646-189">Если вы используете созданный прокси, не нужно создать объект подключения в собственном коде, так как созданный код прокси делает это автоматически.</span><span class="sxs-lookup"><span data-stu-id="2f646-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="2f646-190">**Установить подключение (созданный прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="2f646-191">**Установить соединение (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="2f646-192">В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR.</span><span class="sxs-lookup"><span data-stu-id="2f646-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="2f646-193">Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="2f646-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="2f646-194">Обычно вы регистрировать обработчики событий перед вызовом `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="2f646-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="2f646-195">Если вы хотите зарегистрировать некоторые обработчики событий после подключения к, это можно сделать, но необходимо зарегистрировать по крайней мере один из вашей обработчик или обработчики событий перед вызовом `start` метод.</span><span class="sxs-lookup"><span data-stu-id="2f646-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="2f646-196">Одной из причин этого является может существовать множество центров в приложении, что вы не захотите активировать `OnConnected` событий на каждый центр, если только вы собираетесь использовать для одного из них.</span><span class="sxs-lookup"><span data-stu-id="2f646-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="2f646-197">Когда подключение будет установлено, наличие клиентский метод на прокси-сервера концентратора является указывающий SignalR для активации `OnConnected` событий.</span><span class="sxs-lookup"><span data-stu-id="2f646-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="2f646-198">Если не зарегистрировать все обработчики событий, перед вызовом `start` метод, можно вызывать методы на концентраторе, но центра `OnConnected` не будет вызван метод и методы клиента не будет вызываться с сервера.</span><span class="sxs-lookup"><span data-stu-id="2f646-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="2f646-199">$. connection.hub совпадает с объектом этого $.hubConnection() создает</span><span class="sxs-lookup"><span data-stu-id="2f646-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="2f646-200">Как вы видите, в примерах, при использовании созданного прокси `$.connection.hub` ссылается на объект подключения.</span><span class="sxs-lookup"><span data-stu-id="2f646-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="2f646-201">Это тот же объект, которую можно получить, вызвав `$.hubConnection()` , если не используется созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="2f646-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="2f646-202">Созданный код прокси создает подключение, выполнив следующую инструкцию:</span><span class="sxs-lookup"><span data-stu-id="2f646-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Создание подключения в создаваемого файла посредника](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="2f646-204">Если вы используете созданный прокси-сервер, можно делать все с `$.connection.hub` , выполняемых с помощью объекта соединения при вы не используете созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="2f646-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="2f646-205">Асинхронное выполнение метода start</span><span class="sxs-lookup"><span data-stu-id="2f646-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="2f646-206">`start` Метод выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="2f646-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="2f646-207">Он возвращает [объект jQuery отложенный](http://api.jquery.com/category/deferred-object/), что означает, что можно добавить функции обратного вызова, вызывая методы, такие как `pipe`, `done`, и `fail`.</span><span class="sxs-lookup"><span data-stu-id="2f646-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="2f646-208">Если у вас есть код, который должен выполняться после установки подключения, например вызов метода сервера, помещать код в функцию обратного вызова, или вызывать из функции обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="2f646-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="2f646-209">`.done` Метод обратного вызова выполняется после установления соединения, и после любой код, при наличии вашей `OnConnected` завершении метода обработчика событий на сервере.</span><span class="sxs-lookup"><span data-stu-id="2f646-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="2f646-210">Если поместить оператор «Теперь подключен» из предыдущего примера в следующей строке кода после `start` вызова метода (не в `.done` обратного вызова), `console.log` строка будет иметь прежде, чем подключение будет установлено, как показано в следующем Пример:</span><span class="sxs-lookup"><span data-stu-id="2f646-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Неправильный способ написания кода, который выполняется после установки подключения](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="2f646-212">Как установить подключение между доменами</span><span class="sxs-lookup"><span data-stu-id="2f646-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="2f646-213">Обычно если браузер загружает страницу из `http://contoso.com`, подключении SignalR находится в том же домене, в `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="2f646-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="2f646-214">Если страницы от `http://contoso.com` подключается к `http://fabrikam.com/signalr`, то есть соединение между доменами.</span><span class="sxs-lookup"><span data-stu-id="2f646-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="2f646-215">По соображениям безопасности подключений между доменами отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2f646-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="2f646-216">Для установления соединения между доменами, убедитесь, что междоменные соединения разрешены на сервере и укажите URL-АДРЕСЕ соединения при создании объекта подключения.</span><span class="sxs-lookup"><span data-stu-id="2f646-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="2f646-217">SignalR подходящей технологии для подключения будет использоваться между доменами, такие как [JSONP](http://en.wikipedia.org/wiki/JSONP) или [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="2f646-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="2f646-218">На сервере, включите соединения между доменами, выбрав соответствующий параметр, при вызове `MapHubs` метод.</span><span class="sxs-lookup"><span data-stu-id="2f646-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="2f646-219">На стороне клиента укажите URL-адрес при создании объекта подключения (без созданный прокси-сервера) или перед вызовом метода start (с помощью созданного прокси).</span><span class="sxs-lookup"><span data-stu-id="2f646-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="2f646-220">**Код клиента, который указывает соединение между доменами (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="2f646-221">**Код клиента, который указывает соединение между доменами (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="2f646-222">При использовании `$.hubConnection` конструктор, не обязательно включать `signalr` в URL-АДРЕСЕ, поскольку она добавляется автоматически (если не указано `useDefaultUrl` как `false`).</span><span class="sxs-lookup"><span data-stu-id="2f646-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="2f646-223">Можно создать несколько подключений к другим конечным точкам.</span><span class="sxs-lookup"><span data-stu-id="2f646-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="2f646-224">Не устанавливайте `jQuery.support.cors` значение true в коде.</span><span class="sxs-lookup"><span data-stu-id="2f646-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Не присвоено значение true jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="2f646-226">SignalR обрабатывает использование JSONP или CORS.</span><span class="sxs-lookup"><span data-stu-id="2f646-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="2f646-227">Параметр `jQuery.support.cors` в значение true отключает JSONP, так как вызывает SignalR предположить браузером CORS.</span><span class="sxs-lookup"><span data-stu-id="2f646-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="2f646-228">При подключении к URL-адрес localhost, Internet Explorer 10 не будет считать соединение между доменами, поэтому приложение будет работать локально с помощью Internet Explorer 10 даже если вы не включили междоменных подключений на сервере.</span><span class="sxs-lookup"><span data-stu-id="2f646-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="2f646-229">Сведения об использовании подключений между доменами с Internet Explorer 9, см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="2f646-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="2f646-230">Сведения об использовании подключений между доменами с Chrome, см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="2f646-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="2f646-231">В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR.</span><span class="sxs-lookup"><span data-stu-id="2f646-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="2f646-232">Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="2f646-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="2f646-233">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="2f646-233">How to configure the connection</span></span>

<span data-ttu-id="2f646-234">Перед установкой соединения можно указать параметры строки запроса или указать метод транспорта.</span><span class="sxs-lookup"><span data-stu-id="2f646-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="2f646-235">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="2f646-235">How to specify query string parameters</span></span>

<span data-ttu-id="2f646-236">Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса на объект подключения.</span><span class="sxs-lookup"><span data-stu-id="2f646-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="2f646-237">В следующих примерах для параметра строки запроса в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="2f646-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="2f646-238">**Задайте значения строки запроса перед вызовом метода start (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="2f646-239">**Задайте значения строки запроса перед вызовом метода start (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="2f646-240">Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="2f646-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="2f646-241">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="2f646-241">How to specify the transport method</span></span>

<span data-ttu-id="2f646-242">В рамках процесса подключения клиента SignalR обычно согласовывает с сервером, чтобы определить лучший транспорт, который поддерживается с сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="2f646-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="2f646-243">Если вы уже знаете, какой транспорт, который вы хотите использовать, вы можете обойти этот процесс согласования, указав метод транспорта, при вызове `start` метод.</span><span class="sxs-lookup"><span data-stu-id="2f646-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="2f646-244">**Код клиента, который задает метод передачи (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="2f646-245">**Код клиента, который указывает метод транспорта (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="2f646-246">Кроме того можно указать несколько методов транспорта, в том порядке, в котором SignalR испытать их:</span><span class="sxs-lookup"><span data-stu-id="2f646-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="2f646-247">**Код клиента, который определяет пользовательский транспорт резервный схемы (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="2f646-248">**Код клиента, который определяет резервный схемы пользовательского транспорта (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="2f646-249">Для указания метода транспортировки, можно использовать следующие значения:</span><span class="sxs-lookup"><span data-stu-id="2f646-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="2f646-250">«webSockets»</span><span class="sxs-lookup"><span data-stu-id="2f646-250">"webSockets"</span></span>
- <span data-ttu-id="2f646-251">«foreverFrame»</span><span class="sxs-lookup"><span data-stu-id="2f646-251">"foreverFrame"</span></span>
- <span data-ttu-id="2f646-252">«serverSentEvents»</span><span class="sxs-lookup"><span data-stu-id="2f646-252">"serverSentEvents"</span></span>
- <span data-ttu-id="2f646-253">«longPolling»</span><span class="sxs-lookup"><span data-stu-id="2f646-253">"longPolling"</span></span>

<span data-ttu-id="2f646-254">Как узнать, какой метод транспорта используется соединением, в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="2f646-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="2f646-255">**Код клиента, который отображает метод транспорта, используемый в соединении (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="2f646-256">**Код клиента, который отображает метод транспорта, используемый в соединении (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="2f646-257">Сведения о том, как проверить метод транспорта в серверном коде, см. в разделе [ASP.NET руководство по API концентраторов SignalR - сервер — как для получения сведений о клиенте из контекстного свойства](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="2f646-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="2f646-258">Дополнительные сведения о транспортах и в случае ошибки, см. в разделе [введение в SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="2f646-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="2f646-259">Как получить учетную запись-посредник для класса концентратора</span><span class="sxs-lookup"><span data-stu-id="2f646-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="2f646-260">Каждый объект соединения, который создается инкапсулирует сведения о подключении к службе SignalR, которая содержит один или несколько классов концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f646-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="2f646-261">Для взаимодействия с классом концентратора, использовать прокси-объект созданные пользователем (Если вы не используете созданный прокси-сервера) или которой будет создан автоматически.</span><span class="sxs-lookup"><span data-stu-id="2f646-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="2f646-262">На клиентском компьютере имя прокси-сервера — это версия стиле Camel, имени класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f646-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="2f646-263">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f646-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="2f646-264">**Класс концентратора на сервере**</span><span class="sxs-lookup"><span data-stu-id="2f646-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="2f646-265">**Получить ссылку на созданный прокси клиента для центра**</span><span class="sxs-lookup"><span data-stu-id="2f646-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="2f646-266">**Создание прокси клиента для классу Hub (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="2f646-267">Если вы устанавливаете центр класса с `HubName` атрибута, используйте точное имя без Смена регистра.</span><span class="sxs-lookup"><span data-stu-id="2f646-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="2f646-268">**Класс концентратора на сервере с атрибутом HubName**</span><span class="sxs-lookup"><span data-stu-id="2f646-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="2f646-269">**Получить ссылку на созданный прокси клиента для центра**</span><span class="sxs-lookup"><span data-stu-id="2f646-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="2f646-270">**Создание прокси клиента для классу Hub (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="2f646-271">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="2f646-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="2f646-272">Чтобы определить метод, который сервер может обратиться с помощью центра, добавьте обработчик событий для прокси-сервера концентратора с помощью `client` свойства созданного прокси-сервера, или вызова `on` метод, если вы не используете созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="2f646-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="2f646-273">Параметры могут быть сложными объектами.</span><span class="sxs-lookup"><span data-stu-id="2f646-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="2f646-274">Добавьте обработчик событий, перед вызовом метода `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="2f646-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="2f646-275">(Если вы хотите добавить обработчики событий после вызова метода `start` метод, см. в примечании [как для установления соединения](#establishconnection) выше в этом и используйте синтаксис, показанный для определения метода без использования созданного прокси.)</span><span class="sxs-lookup"><span data-stu-id="2f646-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="2f646-276">Совпадение имен метод не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="2f646-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="2f646-277">Например `Clients.All.addContosoChatMessageToPage` будет выполняться на сервере `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, или `addcontosochatmessagetopage` на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2f646-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="2f646-278">**Определите метод на клиенте (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="2f646-279">**Альтернативный способ определить метод для клиента (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="2f646-280">**Определите метод на клиенте (без созданный прокси-сервера, или при добавлении после вызова метода start)**</span><span class="sxs-lookup"><span data-stu-id="2f646-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="2f646-281">**Код сервера, который вызывает метод клиента**</span><span class="sxs-lookup"><span data-stu-id="2f646-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="2f646-282">Следующие примеры включают сложный объект как параметр метода.</span><span class="sxs-lookup"><span data-stu-id="2f646-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="2f646-283">**Определите метод на клиенте, который принимает сложный объект (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="2f646-284">**Определите метод на клиенте, который принимает сложный объект (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="2f646-285">**Код сервера, который определяет сложный объект**</span><span class="sxs-lookup"><span data-stu-id="2f646-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="2f646-286">**Код сервера, который вызывает метод клиента, используя сложный объект**</span><span class="sxs-lookup"><span data-stu-id="2f646-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="2f646-287">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="2f646-287">How to call server methods from the client</span></span>

<span data-ttu-id="2f646-288">Для вызова серверного метода от клиента, используйте `server` свойства созданного прокси-сервера или `invoke` метод на прокси-концентратора, если вы не используете созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="2f646-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="2f646-289">Возвращаемое значение или параметры могут быть сложными объектами.</span><span class="sxs-lookup"><span data-stu-id="2f646-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="2f646-290">Передать в нижнем регистре версии имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f646-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="2f646-291">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2f646-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="2f646-292">В следующих примерах показано, как вызвать метод сервера, который не имеет возвращаемого значения и способ вызова метода сервера, который имеет возвращаемое значение.</span><span class="sxs-lookup"><span data-stu-id="2f646-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="2f646-293">**Метод сервера без атрибута HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="2f646-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="2f646-294">**Код сервера, который определяет сложный объект, переданный в параметре**</span><span class="sxs-lookup"><span data-stu-id="2f646-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="2f646-295">**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="2f646-296">**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="2f646-297">Если декорировать метод концентратора с `HubMethodName` атрибута, используйте его без Смена регистра.</span><span class="sxs-lookup"><span data-stu-id="2f646-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="2f646-298">**Метод сервера** с атрибутом HubMethodName</span><span class="sxs-lookup"><span data-stu-id="2f646-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="2f646-299">**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="2f646-300">**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="2f646-301">Приведенные выше примеры показывают, как вызвать метод сервера, который не возвращает никакого значения.</span><span class="sxs-lookup"><span data-stu-id="2f646-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="2f646-302">Следующие примеры показывают, как вызвать метод сервера, который имеет возвращаемое значение.</span><span class="sxs-lookup"><span data-stu-id="2f646-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="2f646-303">**Серверный код для метода, который имеет возвращаемое значение**</span><span class="sxs-lookup"><span data-stu-id="2f646-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="2f646-304">**Класс Stock, используемый для** возвращаемое значение</span><span class="sxs-lookup"><span data-stu-id="2f646-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="2f646-305">**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="2f646-306">**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="2f646-307">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="2f646-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="2f646-308">SignalR обеспечивает следующее подключение события времени жизни, которые можно обработать:</span><span class="sxs-lookup"><span data-stu-id="2f646-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="2f646-309">`starting`: Возникает прежде, чем любые данные, отправляются через соединение.</span><span class="sxs-lookup"><span data-stu-id="2f646-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="2f646-310">`received`: Возникает при получении данных для соединения.</span><span class="sxs-lookup"><span data-stu-id="2f646-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="2f646-311">Предоставляет полученных данных.</span><span class="sxs-lookup"><span data-stu-id="2f646-311">Provides the received data.</span></span>
- <span data-ttu-id="2f646-312">`connectionSlow`: Возникает, когда клиент обнаруживает подключение медленно или часто удаление.</span><span class="sxs-lookup"><span data-stu-id="2f646-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="2f646-313">`reconnecting`: Возникает, когда используемому транспорту начинает повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="2f646-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="2f646-314">`reconnected`: Возникает, когда была повторно присоединена используемому транспорту.</span><span class="sxs-lookup"><span data-stu-id="2f646-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="2f646-315">`stateChanged`: Возникает при изменении состояния подключения.</span><span class="sxs-lookup"><span data-stu-id="2f646-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="2f646-316">Предоставляет состояние старое и новое состояние (подключение, подключено, повторное подключение или Disconnected).</span><span class="sxs-lookup"><span data-stu-id="2f646-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="2f646-317">`disconnected`: Возникает, когда произойдет отключение соединения.</span><span class="sxs-lookup"><span data-stu-id="2f646-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="2f646-318">Например, если вы хотите отображать предупреждающие сообщения, при наличии проблем подключения, которые могут привести к значительным задержкам, обрабатывать `connectionSlow` событий.</span><span class="sxs-lookup"><span data-stu-id="2f646-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="2f646-319">**Обработать событие connectionSlow (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="2f646-320">**Обработать событие connectionSlow (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="2f646-321">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="2f646-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="2f646-322">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="2f646-322">How to handle errors</span></span>

<span data-ttu-id="2f646-323">Клиент SignalR JavaScript предоставляет `error` , которые можно добавить обработчик для события.</span><span class="sxs-lookup"><span data-stu-id="2f646-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="2f646-324">Метод fail также можно добавить обработчик для ошибки, возникающие в результате вызова метода сервера.</span><span class="sxs-lookup"><span data-stu-id="2f646-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="2f646-325">Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке.</span><span class="sxs-lookup"><span data-stu-id="2f646-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="2f646-326">Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Отправка подробных сообщений об ошибках клиентов в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.</span><span class="sxs-lookup"><span data-stu-id="2f646-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="2f646-327">В следующем примере показано, как добавить обработчик для события ошибки.</span><span class="sxs-lookup"><span data-stu-id="2f646-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="2f646-328">**Добавить обработчик ошибок (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="2f646-329">**Добавить обработчик ошибок (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="2f646-330">В следующем примере показано, как обработать ошибку из вызова метода.</span><span class="sxs-lookup"><span data-stu-id="2f646-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="2f646-331">**Обработать ошибку из вызова метода (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="2f646-332">**Обработать ошибку из вызова метода (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="2f646-333">В случае сбоя вызова метода `error` также события, поэтому в коде `error` метод обработчика и в `.fail` выполняется метод обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="2f646-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="2f646-334">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="2f646-334">How to enable client-side logging</span></span>

<span data-ttu-id="2f646-335">Чтобы включить ведение журнала на стороне клиента для подключения, задайте `logging` свойство в объекте подключения, перед вызовом метода `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="2f646-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="2f646-336">**Включить ведение журнала (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="2f646-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="2f646-337">**Включение ведения журнала (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="2f646-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="2f646-338">Чтобы просмотреть журналы, откройте средства разработчика браузера и перейдите на вкладку консоли. Учебник, в котором показаны пошаговые инструкции и экрана снимков, которые показывают, как это сделать, см. в разделе [передача сообщений с сервера с помощью Signalr - включить ведение журнала](index.md).</span><span class="sxs-lookup"><span data-stu-id="2f646-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
