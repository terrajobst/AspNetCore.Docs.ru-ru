---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Руководство по API концентраторов ASP.NET SignalR — клиент .NET (C#) | Документация Майкрософт
author: pfletcher
description: Этот документ представляет собой введение по API концентраторов SignalR версии 2 в таких клиентов .NET, Windows Store (WinRT), WPF, Silverlight и «против»...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8ee3be32af6794dd352cdadb668fc0fbba70d815
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369293"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="66a4d-103">Руководство по API концентраторов ASP.NET SignalR — клиент .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="66a4d-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="66a4d-104">по [Флетчера Патрик](https://github.com/pfletcher), [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="66a4d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="66a4d-105">Этот документ содержит вводные сведения по API концентраторов SignalR версии 2 в таких клиентов .NET, Windows Store (WinRT), WPF, Silverlight и консольных приложений.</span><span class="sxs-lookup"><span data-stu-id="66a4d-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="66a4d-106">API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу.</span><span class="sxs-lookup"><span data-stu-id="66a4d-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="66a4d-107">В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="66a4d-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="66a4d-108">В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере.</span><span class="sxs-lookup"><span data-stu-id="66a4d-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="66a4d-109">SignalR берет на себя все необходимое для вас клиент сервер.</span><span class="sxs-lookup"><span data-stu-id="66a4d-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="66a4d-110">SignalR также предлагает API низкого уровня, вызывается постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="66a4d-111">Введение в SignalR, концентраторы и постоянные подключения, или в этом учебнике показано, как создать полное приложение SignalR, см. в разделе [SignalR — Приступая к работе](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="66a4d-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="66a4d-112">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="66a4d-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="66a4d-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="66a4d-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="66a4d-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="66a4d-114">.NET 4.5</span></span>
> - <span data-ttu-id="66a4d-115">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="66a4d-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="66a4d-116">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="66a4d-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="66a4d-117">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="66a4d-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="66a4d-118">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="66a4d-118">Questions and comments</span></span>
> 
> <span data-ttu-id="66a4d-119">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="66a4d-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="66a4d-120">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="66a4d-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="66a4d-121">Обзор</span><span class="sxs-lookup"><span data-stu-id="66a4d-121">Overview</span></span>

<span data-ttu-id="66a4d-122">Этот документ содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="66a4d-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="66a4d-123">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="66a4d-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="66a4d-124">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="66a4d-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="66a4d-125">Междоменные подключения от клиентов Silverlight</span><span class="sxs-lookup"><span data-stu-id="66a4d-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="66a4d-126">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="66a4d-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="66a4d-127">Как задать максимальное число одновременных подключений клиентов WPF</span><span class="sxs-lookup"><span data-stu-id="66a4d-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="66a4d-128">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="66a4d-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="66a4d-129">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="66a4d-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="66a4d-130">Как указать заголовки HTTP</span><span class="sxs-lookup"><span data-stu-id="66a4d-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="66a4d-131">Практическое задание сертификатов клиентов</span><span class="sxs-lookup"><span data-stu-id="66a4d-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="66a4d-132">Как создать прокси-сервера концентратора</span><span class="sxs-lookup"><span data-stu-id="66a4d-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="66a4d-133">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="66a4d-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="66a4d-134">Методы без параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="66a4d-135">Методы с параметрами, указав типы параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="66a4d-136">Методы с параметрами, указав динамические объекты для параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="66a4d-137">Как удалить обработчик</span><span class="sxs-lookup"><span data-stu-id="66a4d-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="66a4d-138">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="66a4d-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="66a4d-139">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="66a4d-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="66a4d-140">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="66a4d-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="66a4d-141">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="66a4d-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="66a4d-142">WPF, Silverlight и образцов кода консольных приложений для клиентских методов, которые могут вызывать сервера</span><span class="sxs-lookup"><span data-stu-id="66a4d-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="66a4d-143">Для образцов проектов клиента .NET ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="66a4d-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="66a4d-144">[Андрей armenta / SignalR — примеры](https://github.com/gustavo-armenta/SignalR-Samples) на сайте GitHub.com (примеры приложений WinRT, Silverlight, консоли).</span><span class="sxs-lookup"><span data-stu-id="66a4d-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="66a4d-145">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) на сайте GitHub.com (например, WPF).</span><span class="sxs-lookup"><span data-stu-id="66a4d-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="66a4d-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) на сайте GitHub.com (например, приложение консоли).</span><span class="sxs-lookup"><span data-stu-id="66a4d-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="66a4d-147">Документацию по программированию сервера или клиентов JavaScript, ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="66a4d-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="66a4d-148">Руководство по API концентраторов SignalR - сервер</span><span class="sxs-lookup"><span data-stu-id="66a4d-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="66a4d-149">Руководство по API концентраторов SignalR — клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="66a4d-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="66a4d-150">Приведены ссылки на разделы, справочник по API .NET 4.5 версия API.</span><span class="sxs-lookup"><span data-stu-id="66a4d-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="66a4d-151">Если вы используете .NET 4, см. в разделе [версия .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="66a4d-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="66a4d-152">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="66a4d-152">Client setup</span></span>

<span data-ttu-id="66a4d-153">Установка [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) пакет NuGet (не [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) пакета).</span><span class="sxs-lookup"><span data-stu-id="66a4d-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="66a4d-154">Этот пакет поддерживает WinRT, Silverlight, WPF, консольного приложения и клиенты Windows Phone, для .NET 4 и .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="66a4d-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="66a4d-155">Если версия SignalR, у вас на стороне клиента отличается от версии, у вас есть на сервере, SignalR часто имеет возможность адаптироваться к разницу.</span><span class="sxs-lookup"><span data-stu-id="66a4d-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="66a4d-156">Например сервере под управлением SignalR версии 2 будет поддерживать клиентов, имеющих установлен 1.1.x, а также клиенты, имеющие версии 2 установлен.</span><span class="sxs-lookup"><span data-stu-id="66a4d-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="66a4d-157">Если разница между версию на сервере и на клиентском компьютере слишком велико, или если клиент является более новой версии сервера, SignalR выдает `InvalidOperationException` исключение, когда клиент пытается установить соединение.</span><span class="sxs-lookup"><span data-stu-id="66a4d-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="66a4d-158">Сообщение об ошибке "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`«.</span><span class="sxs-lookup"><span data-stu-id="66a4d-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="66a4d-159">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="66a4d-159">How to establish a connection</span></span>

<span data-ttu-id="66a4d-160">Чтобы установить соединение, то необходимо создать `HubConnection` объект и создать учетную запись-посредник.</span><span class="sxs-lookup"><span data-stu-id="66a4d-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="66a4d-161">Чтобы установить подключение, вызов `Start` метод `HubConnection` объекта.</span><span class="sxs-lookup"><span data-stu-id="66a4d-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="66a4d-162">Для клиентов JavaScript, вам необходимо зарегистрировать по крайней мере один обработчик событий, перед вызовом `Start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="66a4d-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="66a4d-163">Это не является обязательным для клиентов .NET.</span><span class="sxs-lookup"><span data-stu-id="66a4d-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="66a4d-164">Для клиентов JavaScript, созданный код прокси автоматически создает прокси-серверы для всех концентраторов, которые существуют на сервере, и регистрация обработчика — указывает, какие центры клиент планирует использовать.</span><span class="sxs-lookup"><span data-stu-id="66a4d-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="66a4d-165">Но для клиента .NET создать прокси-серверы центра вручную, поэтому SignalR предполагается, что вы будете использовать любой центр, создать прокси для.</span><span class="sxs-lookup"><span data-stu-id="66a4d-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="66a4d-166">В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR.</span><span class="sxs-lookup"><span data-stu-id="66a4d-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="66a4d-167">Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="66a4d-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="66a4d-168">`Start` Метод выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="66a4d-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="66a4d-169">Чтобы убедиться, что последующие строки кода, которые не выполняются до после установления соединения, используйте `await` в асинхронном методе ASP.NET 4.5 или `.Wait()` в синхронный метод.</span><span class="sxs-lookup"><span data-stu-id="66a4d-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="66a4d-170">Не используйте `.Wait()` в клиенте WinRT.</span><span class="sxs-lookup"><span data-stu-id="66a4d-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="66a4d-171">Междоменные подключения от клиентов Silverlight</span><span class="sxs-lookup"><span data-stu-id="66a4d-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="66a4d-172">Сведения о включении междоменные подключения от клиентов Silverlight, см. в разделе [внесения службы через границы домена](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="66a4d-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="66a4d-173">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="66a4d-173">How to configure the connection</span></span>

<span data-ttu-id="66a4d-174">Перед установкой соединения можно указать любой из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="66a4d-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="66a4d-175">Ограничение одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="66a4d-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="66a4d-176">Параметры строки запроса.</span><span class="sxs-lookup"><span data-stu-id="66a4d-176">Query string parameters.</span></span>
- <span data-ttu-id="66a4d-177">Метод транспорта.</span><span class="sxs-lookup"><span data-stu-id="66a4d-177">The transport method.</span></span>
- <span data-ttu-id="66a4d-178">Заголовки HTTP.</span><span class="sxs-lookup"><span data-stu-id="66a4d-178">HTTP headers.</span></span>
- <span data-ttu-id="66a4d-179">Сертификаты клиента.</span><span class="sxs-lookup"><span data-stu-id="66a4d-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="66a4d-180">Как задать максимальное число одновременных подключений клиентов WPF</span><span class="sxs-lookup"><span data-stu-id="66a4d-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="66a4d-181">В WPF клиентов может потребоваться увеличить максимальное число одновременных подключений со значения по умолчанию 2.</span><span class="sxs-lookup"><span data-stu-id="66a4d-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="66a4d-182">Рекомендуемое значение — 10.</span><span class="sxs-lookup"><span data-stu-id="66a4d-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="66a4d-183">Дополнительные сведения см. в разделе [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="66a4d-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="66a4d-184">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="66a4d-184">How to specify query string parameters</span></span>

<span data-ttu-id="66a4d-185">Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса на объект подключения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="66a4d-186">В следующем примере показано, как параметра строки запроса в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="66a4d-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="66a4d-187">Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="66a4d-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="66a4d-188">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="66a4d-188">How to specify the transport method</span></span>

<span data-ttu-id="66a4d-189">В рамках процесса подключения клиента SignalR обычно согласовывает с сервером, чтобы определить лучший транспорт, который поддерживается с сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="66a4d-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="66a4d-190">Если вы уже знаете, какой транспорт, который вы хотите использовать, вы можете обойти этот процесс согласования.</span><span class="sxs-lookup"><span data-stu-id="66a4d-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="66a4d-191">Чтобы указать метод транспорта, передайте объект транспорта к методу Start.</span><span class="sxs-lookup"><span data-stu-id="66a4d-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="66a4d-192">Приведенный ниже показано, как указать метод транспорта в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="66a4d-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="66a4d-193">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) пространство имен включает следующие классы, которые можно использовать для указания транспортного уровня.</span><span class="sxs-lookup"><span data-stu-id="66a4d-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="66a4d-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="66a4d-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="66a4d-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="66a4d-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="66a4d-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (доступен только в том случае, когда сервер и клиент использовать .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="66a4d-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="66a4d-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (автоматически выбирает лучший транспорт, который поддерживается клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="66a4d-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="66a4d-198">Это транспорта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="66a4d-198">This is the default transport.</span></span> <span data-ttu-id="66a4d-199">Передача ее в `Start` метод имеет тот же эффект, что не передавая никаких действий.)</span><span class="sxs-lookup"><span data-stu-id="66a4d-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="66a4d-200">ForeverFrame транспорта не включен в этот список, так как он используется только в браузерах.</span><span class="sxs-lookup"><span data-stu-id="66a4d-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="66a4d-201">Сведения о том, как проверить метод транспорта в серверном коде, см. в разделе [ASP.NET руководство по API концентраторов SignalR - сервер — как для получения сведений о клиенте из контекстного свойства](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="66a4d-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="66a4d-202">Дополнительные сведения о транспортах и в случае ошибки, см. в разделе [введение в SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="66a4d-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="66a4d-203">Как указать заголовки HTTP</span><span class="sxs-lookup"><span data-stu-id="66a4d-203">How to specify HTTP headers</span></span>

<span data-ttu-id="66a4d-204">Чтобы задать заголовки HTTP, используйте `Headers` свойство для объекта connection.</span><span class="sxs-lookup"><span data-stu-id="66a4d-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="66a4d-205">Приведенный ниже показано, как добавить заголовок HTTP.</span><span class="sxs-lookup"><span data-stu-id="66a4d-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="66a4d-206">Практическое задание сертификатов клиентов</span><span class="sxs-lookup"><span data-stu-id="66a4d-206">How to specify client certificates</span></span>

<span data-ttu-id="66a4d-207">Чтобы добавить сертификаты клиента, используйте `AddClientCertificate` метод для объекта connection.</span><span class="sxs-lookup"><span data-stu-id="66a4d-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="66a4d-208">Как создать прокси-сервера концентратора</span><span class="sxs-lookup"><span data-stu-id="66a4d-208">How to create the Hub proxy</span></span>

<span data-ttu-id="66a4d-209">Чтобы определить методы на клиенте, который можно вызвать концентратор с сервера, а также вызывать методы концентратора на сервере, создать прокси-сервер для центра, вызвав `CreateHubProxy` для объекта connection.</span><span class="sxs-lookup"><span data-stu-id="66a4d-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="66a4d-210">Строка передается в `CreateHubProxy` — это имя класса концентратора или имя, указанное в `HubName` атрибут, если план, который использовался на сервере.</span><span class="sxs-lookup"><span data-stu-id="66a4d-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="66a4d-211">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="66a4d-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="66a4d-212">**Класс концентратора на сервере**</span><span class="sxs-lookup"><span data-stu-id="66a4d-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="66a4d-213">**Создайте клиентский прокси для класса концентратора**</span><span class="sxs-lookup"><span data-stu-id="66a4d-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="66a4d-214">Если вы устанавливаете центр класса с `HubName` атрибута, используйте его.</span><span class="sxs-lookup"><span data-stu-id="66a4d-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="66a4d-215">**Класс концентратора на сервере**</span><span class="sxs-lookup"><span data-stu-id="66a4d-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="66a4d-216">**Создайте клиентский прокси для класса концентратора**</span><span class="sxs-lookup"><span data-stu-id="66a4d-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="66a4d-217">При вызове метода `HubConnection.CreateHubProxy` несколько раз с тем же `hubName`, будут получены такие же кэшируются `IHubProxy` объекта.</span><span class="sxs-lookup"><span data-stu-id="66a4d-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="66a4d-218">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="66a4d-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="66a4d-219">Чтобы определить метод, который сервер может обратиться, использовать прокси-сервер `On` метод, чтобы зарегистрировать обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="66a4d-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="66a4d-220">Совпадение имен метод не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="66a4d-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="66a4d-221">Например `Clients.All.UpdateStockPrice` будет выполняться на сервере `updateStockPrice`, `updatestockprice`, или `UpdateStockPrice` на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="66a4d-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="66a4d-222">Разные клиентские платформы имеют различные требования к как написать код метода для обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="66a4d-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="66a4d-223">Примеры, приведенные предназначены для клиентов WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="66a4d-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="66a4d-224">WPF, Silverlight и примеры приложений консоли, указанные в [отдельном разделе этой статьи](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="66a4d-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="66a4d-225">Методы без параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-225">Methods without parameters</span></span>

<span data-ttu-id="66a4d-226">Если метод обработка не имеет параметров, используйте перегрузку метода нестандартную `On` метод:</span><span class="sxs-lookup"><span data-stu-id="66a4d-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="66a4d-227">**Серверный код вызова клиентского метода без параметров**</span><span class="sxs-lookup"><span data-stu-id="66a4d-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="66a4d-228">**WinRT клиентский код для метода вызывается с сервера без параметров ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="66a4d-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="66a4d-229">Методы с параметрами, указав типы параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="66a4d-230">Если метод обработка имеет параметры, укажите типы параметров, как универсальные типы `On` метод.</span><span class="sxs-lookup"><span data-stu-id="66a4d-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="66a4d-231">Существуют универсальные перегрузки `On` метод, чтобы можно было указать параметры до 8 (4 на Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="66a4d-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="66a4d-232">В следующем примере передается один параметр `UpdateStockPrice` метод.</span><span class="sxs-lookup"><span data-stu-id="66a4d-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="66a4d-233">**Серверный код вызова клиентского метода с параметром**</span><span class="sxs-lookup"><span data-stu-id="66a4d-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="66a4d-234">**Класс Stock, используемое для параметра**</span><span class="sxs-lookup"><span data-stu-id="66a4d-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="66a4d-235">**WinRT клиентский код для метода вызывается с сервера с параметром ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="66a4d-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="66a4d-236">Методы с параметрами, указав динамические объекты для параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="66a4d-237">В качестве альтернативы для задания параметров, универсальных типов `On` метод, параметры можно указать в качестве динамических объектов:</span><span class="sxs-lookup"><span data-stu-id="66a4d-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="66a4d-238">**Серверный код вызова клиентского метода с параметром**</span><span class="sxs-lookup"><span data-stu-id="66a4d-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="66a4d-239">**Класс Stock, используемое для параметра**</span><span class="sxs-lookup"><span data-stu-id="66a4d-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="66a4d-240">**WinRT клиентский код для метода вызывается с сервера с параметром, с помощью динамического объекта для параметра ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="66a4d-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="66a4d-241">Как удалить обработчик</span><span class="sxs-lookup"><span data-stu-id="66a4d-241">How to remove a handler</span></span>

<span data-ttu-id="66a4d-242">Чтобы удалить обработчик, вызовите его `Dispose` метод.</span><span class="sxs-lookup"><span data-stu-id="66a4d-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="66a4d-243">**Клиентский код для метод, вызываемый из сервера**</span><span class="sxs-lookup"><span data-stu-id="66a4d-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="66a4d-244">**Клиентский код для удаления обработчика**</span><span class="sxs-lookup"><span data-stu-id="66a4d-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="66a4d-245">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="66a4d-245">How to call server methods from the client</span></span>

<span data-ttu-id="66a4d-246">Чтобы вызвать метод на сервере, используйте `Invoke` метод на прокси-сервера концентратора.</span><span class="sxs-lookup"><span data-stu-id="66a4d-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="66a4d-247">Если сервер метод не возвращает никакого значения, воспользуйтесь перегрузкой неуниверсальных `Invoke` метод.</span><span class="sxs-lookup"><span data-stu-id="66a4d-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="66a4d-248">**Серверный код для метода, который не имеет возвращаемого значения**</span><span class="sxs-lookup"><span data-stu-id="66a4d-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="66a4d-249">**Код клиента, вызов метода, который не имеет возвращаемого значения**</span><span class="sxs-lookup"><span data-stu-id="66a4d-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="66a4d-250">Если метод сервера имеет возвращаемое значение, укажите тип возвращаемого значения как универсальный тип `Invoke` метод.</span><span class="sxs-lookup"><span data-stu-id="66a4d-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="66a4d-251">**Серверный код для метода, который имеет возвращаемое значение и принимает параметр сложного типа**</span><span class="sxs-lookup"><span data-stu-id="66a4d-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="66a4d-252">**Класс Stock, используемый для параметра и возвращаемого значения**</span><span class="sxs-lookup"><span data-stu-id="66a4d-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="66a4d-253">**Клиентский код, вызвав метод, который имеет возвращаемое значение и принимает параметр сложного типа, в асинхронном методе ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="66a4d-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="66a4d-254">**Клиентский код, вызвав метод, который имеет возвращаемое значение и принимает параметр сложного типа, синхронный метод**</span><span class="sxs-lookup"><span data-stu-id="66a4d-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="66a4d-255">`Invoke` Метод выполняется асинхронно и возвращает `Task` объекта.</span><span class="sxs-lookup"><span data-stu-id="66a4d-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="66a4d-256">Если вы не укажете `await` или `.Wait()`, следующая строка кода будет выполняться до завершения выполнения метода, который вы вызываете.</span><span class="sxs-lookup"><span data-stu-id="66a4d-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="66a4d-257">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="66a4d-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="66a4d-258">SignalR обеспечивает следующее подключение события времени жизни, которые можно обработать:</span><span class="sxs-lookup"><span data-stu-id="66a4d-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="66a4d-259">`Received`: Возникает при получении данных для соединения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="66a4d-260">Предоставляет полученных данных.</span><span class="sxs-lookup"><span data-stu-id="66a4d-260">Provides the received data.</span></span>
- <span data-ttu-id="66a4d-261">`ConnectionSlow`: Возникает, когда клиент обнаруживает подключение медленно или часто удаление.</span><span class="sxs-lookup"><span data-stu-id="66a4d-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="66a4d-262">`Reconnecting`: Возникает, когда используемому транспорту начинает повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="66a4d-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="66a4d-263">`Reconnected`: Возникает, когда была повторно присоединена используемому транспорту.</span><span class="sxs-lookup"><span data-stu-id="66a4d-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="66a4d-264">`StateChanged`: Возникает при изменении состояния подключения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="66a4d-265">Предоставляет состояние старое и новое состояние.</span><span class="sxs-lookup"><span data-stu-id="66a4d-265">Provides the old state and the new state.</span></span> <span data-ttu-id="66a4d-266">Сведения о подключении, значения состояния см. в разделе [перечисление ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="66a4d-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="66a4d-267">`Closed`: Возникает, когда произойдет отключение соединения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="66a4d-268">Например, если вы хотите отображать предупреждающие сообщения для ошибок, которые не являются очень существенными, но вызывают проблемы с промежуточными соединениями, такие как медленная работа или часто удаление соединения, обрабатывать `ConnectionSlow` событий.</span><span class="sxs-lookup"><span data-stu-id="66a4d-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="66a4d-269">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="66a4d-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="66a4d-270">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="66a4d-270">How to handle errors</span></span>

<span data-ttu-id="66a4d-271">Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке.</span><span class="sxs-lookup"><span data-stu-id="66a4d-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="66a4d-272">Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Отправка подробных сообщений об ошибках клиентов в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.</span><span class="sxs-lookup"><span data-stu-id="66a4d-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="66a4d-273">Для обработки ошибок, которые вызывает SignalR, можно добавить обработчик для `Error` событие для объекта подключения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="66a4d-274">Для обработки ошибок из вызовов методов, поместите код в блок try-catch.</span><span class="sxs-lookup"><span data-stu-id="66a4d-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="66a4d-275">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="66a4d-275">How to enable client-side logging</span></span>

<span data-ttu-id="66a4d-276">Чтобы включить ведение журнала на стороне клиента, задайте `TraceLevel` и `TraceWriter` свойства в объекте подключения.</span><span class="sxs-lookup"><span data-stu-id="66a4d-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="66a4d-277">WPF, Silverlight и образцов кода консольных приложений для клиентских методов, которые могут вызывать сервера</span><span class="sxs-lookup"><span data-stu-id="66a4d-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="66a4d-278">Примеры кода, показанный ранее, для определения методов клиента, которые могут вызывать сервера применяются к клиентам WinRT.</span><span class="sxs-lookup"><span data-stu-id="66a4d-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="66a4d-279">В следующих примерах показано эквивалентный код для WPF, Silverlight и консоли приложений-клиентов.</span><span class="sxs-lookup"><span data-stu-id="66a4d-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="66a4d-280">Методы без параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-280">Methods without parameters</span></span>

<span data-ttu-id="66a4d-281">**WPF клиентский код для метода с сервера без параметров**</span><span class="sxs-lookup"><span data-stu-id="66a4d-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="66a4d-282">**Код клиента Silverlight для метод, вызываемый с сервера без параметров**</span><span class="sxs-lookup"><span data-stu-id="66a4d-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="66a4d-283">**Код клиента приложения консоли для метода вызывается с сервера без параметров**</span><span class="sxs-lookup"><span data-stu-id="66a4d-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="66a4d-284">Методы с параметрами, указав типы параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="66a4d-285">**Код клиента WPF для метод, вызываемый из сервера с параметром**</span><span class="sxs-lookup"><span data-stu-id="66a4d-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="66a4d-286">**Код клиента Silverlight для метод, вызываемый из сервера с параметром**</span><span class="sxs-lookup"><span data-stu-id="66a4d-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="66a4d-287">**Код клиента приложения консоли для метода вызывается с сервера с параметром**</span><span class="sxs-lookup"><span data-stu-id="66a4d-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="66a4d-288">Методы с параметрами, указав динамические объекты для параметров</span><span class="sxs-lookup"><span data-stu-id="66a4d-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="66a4d-289">**Код клиента WPF для метод, вызываемый из сервера с параметром, с помощью динамического объекта для параметра**</span><span class="sxs-lookup"><span data-stu-id="66a4d-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="66a4d-290">**Код клиента Silverlight для метод, вызываемый из сервера с параметром, с помощью динамического объекта для параметра**</span><span class="sxs-lookup"><span data-stu-id="66a4d-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="66a4d-291">**Код клиента приложения консоли для метода вызывается с сервера с параметром, с помощью динамического объекта для параметра**</span><span class="sxs-lookup"><span data-stu-id="66a4d-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
