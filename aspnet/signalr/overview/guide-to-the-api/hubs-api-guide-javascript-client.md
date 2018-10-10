---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Руководство по API концентраторов ASP.NET SignalR — клиент JavaScript | Документация Майкрософт
author: pfletcher
description: Этот документ содержит вводные сведения по API концентраторов SignalR версии 2 в клиентах JavaScript, таких как браузеры и applicat Windows Store (WinJS)...
ms.author: riande
ms.date: 09/28/2015
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 9edb7fd100a3f4c5331454045ac206d2f7a81961
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912453"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="0788d-103">Руководство по API концентраторов ASP.NET SignalR — клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="0788d-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="0788d-104">по [Флетчера Патрик](https://github.com/pfletcher), [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0788d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="0788d-105">Этот документ содержит вводные сведения по API концентраторов SignalR версии 2 в клиентах JavaScript, таких как браузеры и приложения Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="0788d-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="0788d-106">API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу.</span><span class="sxs-lookup"><span data-stu-id="0788d-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="0788d-107">В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="0788d-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="0788d-108">В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере.</span><span class="sxs-lookup"><span data-stu-id="0788d-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="0788d-109">SignalR берет на себя все необходимое для вас клиент сервер.</span><span class="sxs-lookup"><span data-stu-id="0788d-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="0788d-110">SignalR также предлагает API низкого уровня, вызывается постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="0788d-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="0788d-111">Введение в SignalR, концентраторы и постоянные подключения, см. в разделе [введение в SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="0788d-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0788d-112">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="0788d-112">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="0788d-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0788d-113">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0788d-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0788d-114">.NET 4.5</span></span>
> - <span data-ttu-id="0788d-115">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="0788d-115">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0788d-116">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="0788d-116">Previous versions of this topic</span></span>
>
> <span data-ttu-id="0788d-117">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0788d-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="0788d-118">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="0788d-118">Questions and comments</span></span>
>
> <span data-ttu-id="0788d-119">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="0788d-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0788d-120">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0788d-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="0788d-121">Обзор</span><span class="sxs-lookup"><span data-stu-id="0788d-121">Overview</span></span>

<span data-ttu-id="0788d-122">Этот документ содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="0788d-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="0788d-123">Созданный прокси и что он делает для вас</span><span class="sxs-lookup"><span data-stu-id="0788d-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="0788d-124">Когда следует использовать созданный прокси</span><span class="sxs-lookup"><span data-stu-id="0788d-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="0788d-125">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="0788d-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="0788d-126">Способ создания ссылок динамически создаваемого прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="0788d-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="0788d-127">Как создать физический файл для SignalR созданный прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="0788d-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="0788d-128">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="0788d-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="0788d-129">$. connection.hub совпадает с объектом этого $.hubConnection() создает</span><span class="sxs-lookup"><span data-stu-id="0788d-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="0788d-130">Асинхронное выполнение метода start</span><span class="sxs-lookup"><span data-stu-id="0788d-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="0788d-131">Как установить подключение между доменами</span><span class="sxs-lookup"><span data-stu-id="0788d-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="0788d-132">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="0788d-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="0788d-133">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="0788d-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="0788d-134">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="0788d-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="0788d-135">Как получить учетную запись-посредник для класса концентратора</span><span class="sxs-lookup"><span data-stu-id="0788d-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="0788d-136">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="0788d-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="0788d-137">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="0788d-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="0788d-138">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="0788d-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="0788d-139">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="0788d-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="0788d-140">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="0788d-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="0788d-141">Документацию по программированию сервера или клиентов .NET, ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="0788d-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="0788d-142">Руководство по API концентраторов SignalR - сервер</span><span class="sxs-lookup"><span data-stu-id="0788d-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="0788d-143">Руководство по API концентраторов SignalR — клиент .NET</span><span class="sxs-lookup"><span data-stu-id="0788d-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="0788d-144">Компонент сервера SignalR 2 доступна в .NET 4.5 (хотя есть клиент .NET для 2 SignalR в .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="0788d-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="0788d-145">Созданный прокси и что он делает для вас</span><span class="sxs-lookup"><span data-stu-id="0788d-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="0788d-146">Можно запрограммировать клиента JavaScript для взаимодействия со службой SignalR с или без прокси-сервер, созданный SignalR для вас.</span><span class="sxs-lookup"><span data-stu-id="0788d-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="0788d-147">Прокси-сервер может сделать для вас является упростить синтаксис кода можно использовать для подключения, методы записи, которые сервер вызывает метод, и вызывать методы на сервере.</span><span class="sxs-lookup"><span data-stu-id="0788d-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="0788d-148">При написании кода для вызова методов сервера, созданного прокси позволяет использовать синтаксис, который выглядит так, будто выполнялись локальной функции: можно написать `serverMethod(arg1, arg2)` вместо `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="0788d-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="0788d-149">Если вы введете имя метода сервера синтаксис созданного прокси позволяет ошибку на стороне клиента будут понятны и окно интерпретации.</span><span class="sxs-lookup"><span data-stu-id="0788d-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="0788d-150">И если вручную создать файл, который определяет прокси-серверы, можно также обеспечить поддержку IntelliSense для написания кода, который вызывает методы сервера.</span><span class="sxs-lookup"><span data-stu-id="0788d-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="0788d-151">Например предположим, что у вас есть следующий класс концентратора на сервере:</span><span class="sxs-lookup"><span data-stu-id="0788d-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="0788d-152">В следующих примерах кода показано, как выглядит код JavaScript для вызова `NewContosoChatMessage` метод на сервере, а также получать вызовы `addContosoChatMessageToPage` метод с сервера.</span><span class="sxs-lookup"><span data-stu-id="0788d-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="0788d-153">**С помощью созданного прокси**</span><span class="sxs-lookup"><span data-stu-id="0788d-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="0788d-154">**Без созданный прокси-сервера**</span><span class="sxs-lookup"><span data-stu-id="0788d-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="0788d-155">Когда следует использовать созданный прокси</span><span class="sxs-lookup"><span data-stu-id="0788d-155">When to use the generated proxy</span></span>

<span data-ttu-id="0788d-156">Если вы хотите зарегистрировать несколько обработчиков событий для метод клиента, сервер вызывает метод, нельзя использовать созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="0788d-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="0788d-157">В противном случае вы можете использовать созданный прокси или не зависимости от используемого языка программирования.</span><span class="sxs-lookup"><span data-stu-id="0788d-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="0788d-158">Если вы решили не использовать его, нет необходимости ссылаться на «/ концентраторов signalr» URL-адрес в `script` элемент в коде клиента.</span><span class="sxs-lookup"><span data-stu-id="0788d-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="0788d-159">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="0788d-159">Client setup</span></span>

<span data-ttu-id="0788d-160">Клиент JavaScript требуются ссылки на jQuery и JavaScript core SignalR.</span><span class="sxs-lookup"><span data-stu-id="0788d-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="0788d-161">1.6.4 или основных более поздних версий, например 1.7.2, 1.8.2 или 1.9.1, требуется версия jQuery.</span><span class="sxs-lookup"><span data-stu-id="0788d-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="0788d-162">Если вы решили использовать созданный прокси, необходимо также ссылку на прокси-сервера SignalR созданный файл JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0788d-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="0788d-163">В следующем примере показано, как может выглядеть ссылки на странице HTML, который использует созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="0788d-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="0788d-164">Эти ссылки должны быть включены в следующем порядке: jQuery, SignalR core после этого и прокси-серверы SignalR Фамилия.</span><span class="sxs-lookup"><span data-stu-id="0788d-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="0788d-165">Способ создания ссылок динамически создаваемого прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="0788d-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="0788d-166">В предыдущем примере ссылка на прокси-сервер создается SignalR — динамически созданный код JavaScript, не к какому файлу.</span><span class="sxs-lookup"><span data-stu-id="0788d-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="0788d-167">SignalR создает код JavaScript для прокси-сервера в режиме реального времени и обслуживает его клиенту в ответ на URL-адрес «/ signalr/концентраторы».</span><span class="sxs-lookup"><span data-stu-id="0788d-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="0788d-168">Если вы указали другой базовый URL-адрес для подключений SignalR на сервере в вашей `MapSignalR` , URL-адрес динамически создаваемого файла посредника является пользовательский URL-адрес с «/ концентраторы» добавленным к нему.</span><span class="sxs-lookup"><span data-stu-id="0788d-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="0788d-169">Для клиентов Windows 8 (Windows Store) JavaScript используйте физической посредника вместо того, динамически создаваемый.</span><span class="sxs-lookup"><span data-stu-id="0788d-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="0788d-170">Дополнительные сведения см. в разделе [прокси, созданного как создать физический файл для SignalR](#manualproxy) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0788d-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="0788d-171">В ASP.NET MVC 4 или 5. Просмотр Razor используйте тильда для обращения к корневой папке приложения в файл справки прокси-сервера:</span><span class="sxs-lookup"><span data-stu-id="0788d-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="0788d-172">Дополнительные сведения об использовании SignalR в MVC 5 см. в разделе [начало работы с SignalR и MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="0788d-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="0788d-173">В представлении Razor в ASP.NET MVC 3, используйте `Url.Content` для прокси-сервера файл справки:</span><span class="sxs-lookup"><span data-stu-id="0788d-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="0788d-174">В приложении веб-форм ASP.NET использовать `ResolveClientUrl` для ваших прокси-серверов, ссылка на файл, или зарегистрировать его с помощью ScriptManager, с помощью приложения корневой путь (начинающийся с тильды):</span><span class="sxs-lookup"><span data-stu-id="0788d-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="0788d-175">Как правило используйте тот же метод для указания URL-адреса с «/ signalr/концентраторы», используемого для файлов CSS или JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0788d-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="0788d-176">При указании URL-адрес без использования тильда в некоторых сценариях приложение будет работать правильно при тестирования в Visual Studio, используя IIS Express, но будут завершаться сообщение об ошибке 404, при развертывании на полноценный сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="0788d-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="0788d-177">Дополнительные сведения см. в разделе **разрешении ссылок на ресурсы корневого уровня** в [веб-серверов в Visual Studio для веб-проектов ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="0788d-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="0788d-178">При запуске веб-проекта в Visual Studio 2013 в режиме отладки, и если вы используете Internet Explorer как браузер, вы увидите файл прокси в **обозревателе решений** под **документы скриптов**, как показано на на рисунке.</span><span class="sxs-lookup"><span data-stu-id="0788d-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Файл созданного прокси JavaScript в обозревателе решений](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="0788d-180">Чтобы просмотреть содержимое файла, дважды щелкните **концентраторов**.</span><span class="sxs-lookup"><span data-stu-id="0788d-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="0788d-181">Если вы не используете Visual Studio 2012 или 2013 и Internet Explorer или если вы не в режиме отладки, можно также получить содержимое файла, перейдя по адресу «/ signalR/концентраторы» URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="0788d-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="0788d-182">Например, если ваш сайт работает в `http://localhost:56699`, перейдите в меню `http://localhost:56699/SignalR/hubs` в браузере.</span><span class="sxs-lookup"><span data-stu-id="0788d-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="0788d-183">Как создать физический файл для SignalR созданный прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="0788d-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="0788d-184">В качестве альтернативы на динамически созданный прокси-сервер можно создать физический файл, содержащий код прокси-сервера и ссылку на этот файл.</span><span class="sxs-lookup"><span data-stu-id="0788d-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="0788d-185">Вы можете это сделать, для контроля над кэшированием или объединение поведение, или для получения IntelliSense при написании кода вызовы методов сервера.</span><span class="sxs-lookup"><span data-stu-id="0788d-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="0788d-186">Чтобы создать прокси-файл, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="0788d-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="0788d-187">Установка [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="0788d-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="0788d-188">Откройте командную строку и перейдите к *средства* папку, содержащую файл SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="0788d-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="0788d-189">В папку средств находится по следующему адресу:</span><span class="sxs-lookup"><span data-stu-id="0788d-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="0788d-190">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0788d-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="0788d-191">Путь к вашей *.dll* обычно *bin* папку в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="0788d-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="0788d-192">Эта команда создает файл с именем *server.js* в той же папке, что *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="0788d-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="0788d-193">Поместите *server.js* в соответствующую папку в проекте, переименуйте его в зависимости от приложения и добавьте ссылку на него вместо ссылки на «/ концентраторов signalr».</span><span class="sxs-lookup"><span data-stu-id="0788d-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="0788d-194">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="0788d-194">How to establish a connection</span></span>

<span data-ttu-id="0788d-195">Чтобы установить подключение, необходимо создать объект подключения, создать учетную запись-посредник и регистрировать обработчики событий для методов, которые могут вызываться с сервера.</span><span class="sxs-lookup"><span data-stu-id="0788d-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="0788d-196">При настройке прокси-сервера и обработчики событий, установить соединение, вызвав `start` метод.</span><span class="sxs-lookup"><span data-stu-id="0788d-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="0788d-197">Если вы используете созданный прокси, не нужно создать объект подключения в собственном коде, так как созданный код прокси делает это автоматически.</span><span class="sxs-lookup"><span data-stu-id="0788d-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="0788d-198">**Установить подключение (созданный прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="0788d-199">**Установить соединение (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="0788d-200">В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR.</span><span class="sxs-lookup"><span data-stu-id="0788d-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="0788d-201">Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="0788d-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="0788d-202">По умолчанию расположения концентратора является текущим сервером; Если вы подключаетесь к другому серверу, укажите URL-адрес перед вызовом `start` метод, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0788d-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="0788d-203">Обычно вы регистрировать обработчики событий перед вызовом `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="0788d-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="0788d-204">Если вы хотите зарегистрировать некоторые обработчики событий после подключения к, это можно сделать, но необходимо зарегистрировать по крайней мере один из вашей обработчик или обработчики событий перед вызовом `start` метод.</span><span class="sxs-lookup"><span data-stu-id="0788d-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="0788d-205">Одной из причин этого является может существовать множество центров в приложении, что вы не захотите активировать `OnConnected` событий на каждый центр, если только вы собираетесь использовать для одного из них.</span><span class="sxs-lookup"><span data-stu-id="0788d-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="0788d-206">Когда подключение будет установлено, наличие клиентский метод на прокси-сервера концентратора является указывающий SignalR для активации `OnConnected` событий.</span><span class="sxs-lookup"><span data-stu-id="0788d-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="0788d-207">Если не зарегистрировать все обработчики событий, перед вызовом `start` метод, можно вызывать методы на концентраторе, но центра `OnConnected` не будет вызван метод и методы клиента не будет вызываться с сервера.</span><span class="sxs-lookup"><span data-stu-id="0788d-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="0788d-208">$. connection.hub совпадает с объектом этого $.hubConnection() создает</span><span class="sxs-lookup"><span data-stu-id="0788d-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="0788d-209">Как вы видите, в примерах, при использовании созданного прокси `$.connection.hub` ссылается на объект подключения.</span><span class="sxs-lookup"><span data-stu-id="0788d-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="0788d-210">Это тот же объект, которую можно получить, вызвав `$.hubConnection()` , если не используется созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="0788d-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="0788d-211">Созданный код прокси создает подключение, выполнив следующую инструкцию:</span><span class="sxs-lookup"><span data-stu-id="0788d-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Создание подключения в создаваемого файла посредника](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="0788d-213">Если вы используете созданный прокси-сервер, можно делать все с `$.connection.hub` , выполняемых с помощью объекта соединения при вы не используете созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="0788d-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="0788d-214">Асинхронное выполнение метода start</span><span class="sxs-lookup"><span data-stu-id="0788d-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="0788d-215">`start` Метод выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="0788d-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="0788d-216">Он возвращает [объект jQuery отложенный](http://api.jquery.com/category/deferred-object/), что означает, что можно добавить функции обратного вызова, вызывая методы, такие как `pipe`, `done`, и `fail`.</span><span class="sxs-lookup"><span data-stu-id="0788d-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="0788d-217">Если у вас есть код, который должен выполняться после установки подключения, например вызов метода сервера, помещать код в функцию обратного вызова, или вызывать из функции обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="0788d-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="0788d-218">`.done` Метод обратного вызова выполняется после установления соединения, и после любой код, при наличии вашей `OnConnected` завершении метода обработчика событий на сервере.</span><span class="sxs-lookup"><span data-stu-id="0788d-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="0788d-219">Если поместить оператор «Теперь подключен» из предыдущего примера в следующей строке кода после `start` вызова метода (не в `.done` обратного вызова), `console.log` строка будет иметь прежде, чем подключение будет установлено, как показано в следующем Пример:</span><span class="sxs-lookup"><span data-stu-id="0788d-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Неправильный способ написания кода, который выполняется после установки подключения](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="0788d-221">Как установить подключение между доменами</span><span class="sxs-lookup"><span data-stu-id="0788d-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="0788d-222">Обычно если браузер загружает страницу из `http://contoso.com`, подключении SignalR находится в том же домене, в `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="0788d-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="0788d-223">Если страницы от `http://contoso.com` подключается к `http://fabrikam.com/signalr`, то есть соединение между доменами.</span><span class="sxs-lookup"><span data-stu-id="0788d-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="0788d-224">По соображениям безопасности подключений между доменами отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0788d-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="0788d-225">В SignalR 1.x, междоменные запросы были контролируются одного флага EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="0788d-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="0788d-226">Этот флаг контролировать запросы JSONP и CORS.</span><span class="sxs-lookup"><span data-stu-id="0788d-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="0788d-227">Для повышения гибкости, поддерживают все CORS был удален из серверного компонента SignalR (JavaScript клиенты по-прежнему использовать CORS обычно при его обнаружении, что он поддерживает браузер), а новый промежуточный слой OWIN стала доступной для поддержки следующих сценариев.</span><span class="sxs-lookup"><span data-stu-id="0788d-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="0788d-228">Если JSONP является обязательным для клиента (для поддержки запросов между доменами в старых браузерах), его необходимо явно включить, задав `EnableJSONP` на `HubConfiguration` объект `true`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0788d-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="0788d-229">JSONP отключена по умолчанию, так как это менее безопасно, чем CORS.</span><span class="sxs-lookup"><span data-stu-id="0788d-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="0788d-230">**Добавление в проект Microsoft.Owin.Cors:** , установите эту библиотеку, выполните следующую команду в консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="0788d-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="0788d-231">Эта команда добавит 2.1.0 версию пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="0788d-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="0788d-232">Вызов UseCors</span><span class="sxs-lookup"><span data-stu-id="0788d-232">Calling UseCors</span></span>

 <span data-ttu-id="0788d-233">В следующем фрагменте кода демонстрируется реализация междоменных подключений в SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="0788d-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="0788d-234">**Реализация междоменных запросов в SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="0788d-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="0788d-235">Следующий код демонстрирует включение CORS и JSONP в проекте SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="0788d-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="0788d-236">Этот пример кода использует `Map` и `RunSignalR` вместо `MapSignalR`, таким образом, по промежуточного слоя CORS выполнялось только для запросов SignalR, которым требуется поддержка CORS (а не для всего трафика по пути, указанному в `MapSignalR`.) Карта может также использоваться для любого другого по промежуточного слоя, должен быть запущен для указанного префикса URL-адрес, а не для всего приложения.</span><span class="sxs-lookup"><span data-stu-id="0788d-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="0788d-237">Не устанавливайте `jQuery.support.cors` значение true в коде.</span><span class="sxs-lookup"><span data-stu-id="0788d-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Не присвоено значение true jQuery.support.cors](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="0788d-239">SignalR обрабатывает использование CORS.</span><span class="sxs-lookup"><span data-stu-id="0788d-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="0788d-240">Параметр `jQuery.support.cors` в значение true отключает JSONP, так как вызывает SignalR предположить браузером CORS.</span><span class="sxs-lookup"><span data-stu-id="0788d-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="0788d-241">При подключении к URL-адрес localhost, Internet Explorer 10 не будет считать соединение между доменами, поэтому приложение будет работать локально с помощью Internet Explorer 10 даже если вы не включили междоменных подключений на сервере.</span><span class="sxs-lookup"><span data-stu-id="0788d-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="0788d-242">Сведения об использовании подключений между доменами с Internet Explorer 9, см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="0788d-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="0788d-243">Сведения об использовании подключений между доменами с Chrome, см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="0788d-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="0788d-244">В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR.</span><span class="sxs-lookup"><span data-stu-id="0788d-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="0788d-245">Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="0788d-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="0788d-246">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="0788d-246">How to configure the connection</span></span>

<span data-ttu-id="0788d-247">Перед установкой соединения можно указать параметры строки запроса или указать метод транспорта.</span><span class="sxs-lookup"><span data-stu-id="0788d-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="0788d-248">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="0788d-248">How to specify query string parameters</span></span>

<span data-ttu-id="0788d-249">Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса на объект подключения.</span><span class="sxs-lookup"><span data-stu-id="0788d-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="0788d-250">В следующих примерах для параметра строки запроса в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="0788d-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="0788d-251">**Задайте значения строки запроса перед вызовом метода start (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="0788d-252">**Задайте значения строки запроса перед вызовом метода start (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="0788d-253">Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="0788d-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="0788d-254">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="0788d-254">How to specify the transport method</span></span>

<span data-ttu-id="0788d-255">В рамках процесса подключения клиента SignalR обычно согласовывает с сервером, чтобы определить лучший транспорт, который поддерживается с сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="0788d-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="0788d-256">Если вы уже знаете, какой транспорт, который вы хотите использовать, вы можете обойти этот процесс согласования, указав метод транспорта, при вызове `start` метод.</span><span class="sxs-lookup"><span data-stu-id="0788d-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="0788d-257">**Код клиента, который задает метод передачи (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="0788d-258">**Код клиента, который указывает метод транспорта (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="0788d-259">Кроме того можно указать несколько методов транспорта, в том порядке, в котором SignalR испытать их:</span><span class="sxs-lookup"><span data-stu-id="0788d-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="0788d-260">**Код клиента, который определяет пользовательский транспорт резервный схемы (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="0788d-261">**Код клиента, который определяет резервный схемы пользовательского транспорта (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="0788d-262">Для указания метода транспортировки, можно использовать следующие значения:</span><span class="sxs-lookup"><span data-stu-id="0788d-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="0788d-263">«webSockets»</span><span class="sxs-lookup"><span data-stu-id="0788d-263">"webSockets"</span></span>
- <span data-ttu-id="0788d-264">«foreverFrame»</span><span class="sxs-lookup"><span data-stu-id="0788d-264">"foreverFrame"</span></span>
- <span data-ttu-id="0788d-265">«serverSentEvents»</span><span class="sxs-lookup"><span data-stu-id="0788d-265">"serverSentEvents"</span></span>
- <span data-ttu-id="0788d-266">«longPolling»</span><span class="sxs-lookup"><span data-stu-id="0788d-266">"longPolling"</span></span>

<span data-ttu-id="0788d-267">Как узнать, какой метод транспорта используется соединением, в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="0788d-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="0788d-268">**Код клиента, который отображает метод транспорта, используемый в соединении (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="0788d-269">**Код клиента, который отображает метод транспорта, используемый в соединении (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="0788d-270">Сведения о том, как проверить метод транспорта в серверном коде, см. в разделе [ASP.NET руководство по API концентраторов SignalR - сервер — как для получения сведений о клиенте из контекстного свойства](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="0788d-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="0788d-271">Дополнительные сведения о транспортах и в случае ошибки, см. в разделе [введение в SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="0788d-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="0788d-272">Как получить учетную запись-посредник для класса концентратора</span><span class="sxs-lookup"><span data-stu-id="0788d-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="0788d-273">Каждый объект соединения, который создается инкапсулирует сведения о подключении к службе SignalR, которая содержит один или несколько классов концентратора.</span><span class="sxs-lookup"><span data-stu-id="0788d-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="0788d-274">Для взаимодействия с классом концентратора, использовать прокси-объект созданные пользователем (Если вы не используете созданный прокси-сервера) или которой будет создан автоматически.</span><span class="sxs-lookup"><span data-stu-id="0788d-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="0788d-275">На клиентском компьютере имя прокси-сервера — это версия стиле Camel, имени класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="0788d-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="0788d-276">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0788d-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="0788d-277">**Класс концентратора на сервере**</span><span class="sxs-lookup"><span data-stu-id="0788d-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="0788d-278">**Получить ссылку на созданный прокси клиента для центра**</span><span class="sxs-lookup"><span data-stu-id="0788d-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="0788d-279">**Создание прокси клиента для классу Hub (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="0788d-280">Если вы устанавливаете центр класса с `HubName` атрибута, используйте точное имя без Смена регистра.</span><span class="sxs-lookup"><span data-stu-id="0788d-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="0788d-281">**Класс концентратора на сервере с атрибутом HubName**</span><span class="sxs-lookup"><span data-stu-id="0788d-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="0788d-282">**Получить ссылку на созданный прокси клиента для центра**</span><span class="sxs-lookup"><span data-stu-id="0788d-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="0788d-283">**Создание прокси клиента для классу Hub (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="0788d-284">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="0788d-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="0788d-285">Чтобы определить метод, который сервер может обратиться с помощью центра, добавьте обработчик событий для прокси-сервера концентратора с помощью `client` свойства созданного прокси-сервера, или вызова `on` метод, если вы не используете созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="0788d-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="0788d-286">Параметры могут быть сложными объектами.</span><span class="sxs-lookup"><span data-stu-id="0788d-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="0788d-287">Добавьте обработчик событий, перед вызовом метода `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="0788d-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="0788d-288">(Если вы хотите добавить обработчики событий после вызова метода `start` метод, см. в примечании [как для установления соединения](#establishconnection) выше в этом и используйте синтаксис, показанный для определения метода без использования созданного прокси.)</span><span class="sxs-lookup"><span data-stu-id="0788d-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="0788d-289">Совпадение имен метод не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="0788d-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="0788d-290">Например `Clients.All.addContosoChatMessageToPage` будет выполняться на сервере `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, или `addcontosochatmessagetopage` на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0788d-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="0788d-291">**Определите метод на клиенте (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="0788d-292">**Альтернативный способ определить метод для клиента (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="0788d-293">**Определите метод на клиенте (без созданный прокси-сервера, или при добавлении после вызова метода start)**</span><span class="sxs-lookup"><span data-stu-id="0788d-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="0788d-294">**Код сервера, который вызывает метод клиента**</span><span class="sxs-lookup"><span data-stu-id="0788d-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="0788d-295">Следующие примеры включают сложный объект как параметр метода.</span><span class="sxs-lookup"><span data-stu-id="0788d-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="0788d-296">**Определите метод на клиенте, который принимает сложный объект (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="0788d-297">**Определите метод на клиенте, который принимает сложный объект (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="0788d-298">**Код сервера, который определяет сложный объект**</span><span class="sxs-lookup"><span data-stu-id="0788d-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="0788d-299">**Код сервера, который вызывает метод клиента, используя сложный объект**</span><span class="sxs-lookup"><span data-stu-id="0788d-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="0788d-300">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="0788d-300">How to call server methods from the client</span></span>

<span data-ttu-id="0788d-301">Для вызова серверного метода от клиента, используйте `server` свойства созданного прокси-сервера или `invoke` метод на прокси-концентратора, если вы не используете созданный прокси.</span><span class="sxs-lookup"><span data-stu-id="0788d-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="0788d-302">Возвращаемое значение или параметры могут быть сложными объектами.</span><span class="sxs-lookup"><span data-stu-id="0788d-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="0788d-303">Передать в нижнем регистре версии имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="0788d-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="0788d-304">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0788d-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="0788d-305">В следующих примерах показано, как вызвать метод сервера, который не имеет возвращаемого значения и способ вызова метода сервера, который имеет возвращаемое значение.</span><span class="sxs-lookup"><span data-stu-id="0788d-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="0788d-306">**Метод сервера без атрибута HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="0788d-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="0788d-307">**Код сервера, который определяет сложный объект, переданный в параметре**</span><span class="sxs-lookup"><span data-stu-id="0788d-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="0788d-308">**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="0788d-309">**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="0788d-310">Если декорировать метод концентратора с `HubMethodName` атрибута, используйте его без Смена регистра.</span><span class="sxs-lookup"><span data-stu-id="0788d-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="0788d-311">**Метод сервера** с атрибутом HubMethodName</span><span class="sxs-lookup"><span data-stu-id="0788d-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="0788d-312">**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="0788d-313">**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="0788d-314">Приведенные выше примеры показывают, как вызвать метод сервера, который не возвращает никакого значения.</span><span class="sxs-lookup"><span data-stu-id="0788d-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="0788d-315">Следующие примеры показывают, как вызвать метод сервера, который имеет возвращаемое значение.</span><span class="sxs-lookup"><span data-stu-id="0788d-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="0788d-316">**Серверный код для метода, который имеет возвращаемое значение**</span><span class="sxs-lookup"><span data-stu-id="0788d-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="0788d-317">**Класс Stock, используемый для** возвращаемое значение</span><span class="sxs-lookup"><span data-stu-id="0788d-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="0788d-318">**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="0788d-319">**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="0788d-320">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="0788d-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="0788d-321">SignalR обеспечивает следующее подключение события времени жизни, которые можно обработать:</span><span class="sxs-lookup"><span data-stu-id="0788d-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="0788d-322">`starting`: Возникает прежде, чем любые данные, отправляются через соединение.</span><span class="sxs-lookup"><span data-stu-id="0788d-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="0788d-323">`received`: Возникает при получении данных для соединения.</span><span class="sxs-lookup"><span data-stu-id="0788d-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="0788d-324">Предоставляет полученных данных.</span><span class="sxs-lookup"><span data-stu-id="0788d-324">Provides the received data.</span></span>
- <span data-ttu-id="0788d-325">`connectionSlow`: Возникает, когда клиент обнаруживает подключение медленно или часто удаление.</span><span class="sxs-lookup"><span data-stu-id="0788d-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="0788d-326">`reconnecting`: Возникает, когда используемому транспорту начинает повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="0788d-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="0788d-327">`reconnected`: Возникает, когда была повторно присоединена используемому транспорту.</span><span class="sxs-lookup"><span data-stu-id="0788d-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="0788d-328">`stateChanged`: Возникает при изменении состояния подключения.</span><span class="sxs-lookup"><span data-stu-id="0788d-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="0788d-329">Предоставляет состояние старое и новое состояние (подключение, подключено, повторное подключение или Disconnected).</span><span class="sxs-lookup"><span data-stu-id="0788d-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="0788d-330">`disconnected`: Возникает, когда произойдет отключение соединения.</span><span class="sxs-lookup"><span data-stu-id="0788d-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="0788d-331">Например, если вы хотите отображать предупреждающие сообщения, при наличии проблем подключения, которые могут привести к значительным задержкам, обрабатывать `connectionSlow` событий.</span><span class="sxs-lookup"><span data-stu-id="0788d-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="0788d-332">**Обработать событие connectionSlow (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="0788d-333">**Обработать событие connectionSlow (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="0788d-334">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="0788d-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="0788d-335">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="0788d-335">How to handle errors</span></span>

<span data-ttu-id="0788d-336">Клиент SignalR JavaScript предоставляет `error` , которые можно добавить обработчик для события.</span><span class="sxs-lookup"><span data-stu-id="0788d-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="0788d-337">Метод fail также можно добавить обработчик для ошибки, возникающие в результате вызова метода сервера.</span><span class="sxs-lookup"><span data-stu-id="0788d-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="0788d-338">Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке.</span><span class="sxs-lookup"><span data-stu-id="0788d-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="0788d-339">Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Отправка подробных сообщений об ошибках клиентов в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.</span><span class="sxs-lookup"><span data-stu-id="0788d-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="0788d-340">В следующем примере показано, как добавить обработчик для события ошибки.</span><span class="sxs-lookup"><span data-stu-id="0788d-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="0788d-341">**Добавить обработчик ошибок (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="0788d-342">**Добавить обработчик ошибок (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="0788d-343">В следующем примере показано, как обработать ошибку из вызова метода.</span><span class="sxs-lookup"><span data-stu-id="0788d-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="0788d-344">**Обработать ошибку из вызова метода (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="0788d-345">**Обработать ошибку из вызова метода (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="0788d-346">В случае сбоя вызова метода `error` также события, поэтому в коде `error` метод обработчика и в `.fail` выполняется метод обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="0788d-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="0788d-347">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="0788d-347">How to enable client-side logging</span></span>

<span data-ttu-id="0788d-348">Чтобы включить ведение журнала на стороне клиента для подключения, задайте `logging` свойство в объекте подключения, перед вызовом метода `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="0788d-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="0788d-349">**Включить ведение журнала (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="0788d-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="0788d-350">**Включение ведения журнала (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="0788d-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="0788d-351">Чтобы просмотреть журналы, откройте средства разработчика браузера и перейдите на вкладку консоли. Учебник, в котором показаны пошаговые инструкции и экрана снимков, которые показывают, как это сделать, см. в разделе [передача сообщений с сервера с помощью Signalr - включить ведение журнала](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="0788d-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
