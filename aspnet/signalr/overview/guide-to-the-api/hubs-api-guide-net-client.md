---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Руководство по API концентраторов ASP.NET SignalR — клиент .NET (C#) | Документация Майкрософт
author: pfletcher
description: Этот документ представляет собой введение по API концентраторов SignalR версии 2 в таких клиентов .NET, Windows Store (WinRT), WPF, Silverlight и «против»...
ms.author: riande
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9981b8b91be3395b1a3aa7e0cabb1b7f455d47be
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396185"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="8657d-103">Руководство по API концентраторов ASP.NET SignalR — клиент .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="8657d-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="8657d-104">Этот документ содержит вводные сведения по API концентраторов SignalR версии 2 в таких клиентов .NET, Windows Store (WinRT), WPF, Silverlight и консольных приложений.</span><span class="sxs-lookup"><span data-stu-id="8657d-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="8657d-105">API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу.</span><span class="sxs-lookup"><span data-stu-id="8657d-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="8657d-106">В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="8657d-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="8657d-107">В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере.</span><span class="sxs-lookup"><span data-stu-id="8657d-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="8657d-108">SignalR берет на себя все необходимое для вас клиент сервер.</span><span class="sxs-lookup"><span data-stu-id="8657d-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="8657d-109">SignalR также предлагает API низкого уровня, вызывается постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="8657d-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="8657d-110">Введение в SignalR, концентраторы и постоянные подключения, или в этом учебнике показано, как создать полное приложение SignalR, см. в разделе [SignalR — Приступая к работе](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="8657d-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8657d-111">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="8657d-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="8657d-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8657d-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="8657d-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8657d-113">.NET 4.5</span></span>
> - <span data-ttu-id="8657d-114">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="8657d-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8657d-115">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="8657d-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="8657d-116">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8657d-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="8657d-117">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="8657d-117">Questions and comments</span></span>
>
> <span data-ttu-id="8657d-118">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="8657d-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8657d-119">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8657d-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="8657d-120">Обзор</span><span class="sxs-lookup"><span data-stu-id="8657d-120">Overview</span></span>

<span data-ttu-id="8657d-121">Этот документ содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="8657d-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="8657d-122">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="8657d-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="8657d-123">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="8657d-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="8657d-124">Междоменные подключения от клиентов Silverlight</span><span class="sxs-lookup"><span data-stu-id="8657d-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="8657d-125">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="8657d-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="8657d-126">Как задать максимальное число одновременных подключений клиентов WPF</span><span class="sxs-lookup"><span data-stu-id="8657d-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="8657d-127">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="8657d-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="8657d-128">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="8657d-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="8657d-129">Как указать заголовки HTTP</span><span class="sxs-lookup"><span data-stu-id="8657d-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="8657d-130">Практическое задание сертификатов клиентов</span><span class="sxs-lookup"><span data-stu-id="8657d-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="8657d-131">Как создать прокси-сервера концентратора</span><span class="sxs-lookup"><span data-stu-id="8657d-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="8657d-132">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="8657d-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="8657d-133">Методы без параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="8657d-134">Методы с параметрами, указав типы параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="8657d-135">Методы с параметрами, указав динамические объекты для параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="8657d-136">Как удалить обработчик</span><span class="sxs-lookup"><span data-stu-id="8657d-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="8657d-137">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="8657d-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="8657d-138">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="8657d-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="8657d-139">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="8657d-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="8657d-140">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="8657d-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="8657d-141">WPF, Silverlight и образцов кода консольных приложений для клиентских методов, которые могут вызывать сервера</span><span class="sxs-lookup"><span data-stu-id="8657d-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="8657d-142">Для образцов проектов клиента .NET ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="8657d-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="8657d-143">[Андрей armenta / SignalR — примеры](https://github.com/gustavo-armenta/SignalR-Samples) на сайте GitHub.com (примеры приложений WinRT, Silverlight, консоли).</span><span class="sxs-lookup"><span data-stu-id="8657d-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="8657d-144">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) на сайте GitHub.com (например, WPF).</span><span class="sxs-lookup"><span data-stu-id="8657d-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="8657d-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) на сайте GitHub.com (например, приложение консоли).</span><span class="sxs-lookup"><span data-stu-id="8657d-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="8657d-146">Документацию по программированию сервера или клиентов JavaScript, ознакомьтесь со следующими ресурсами:</span><span class="sxs-lookup"><span data-stu-id="8657d-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="8657d-147">Руководство по API концентраторов SignalR - сервер</span><span class="sxs-lookup"><span data-stu-id="8657d-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="8657d-148">Руководство по API концентраторов SignalR — клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="8657d-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="8657d-149">Приведены ссылки на разделы, справочник по API .NET 4.5 версия API.</span><span class="sxs-lookup"><span data-stu-id="8657d-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="8657d-150">Если вы используете .NET 4, см. в разделе [версия .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="8657d-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="8657d-151">Установка клиента</span><span class="sxs-lookup"><span data-stu-id="8657d-151">Client setup</span></span>

<span data-ttu-id="8657d-152">Установка [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) пакет NuGet (не [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) пакета).</span><span class="sxs-lookup"><span data-stu-id="8657d-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="8657d-153">Этот пакет поддерживает WinRT, Silverlight, WPF, консольного приложения и клиенты Windows Phone, для .NET 4 и .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="8657d-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="8657d-154">Если версия SignalR, у вас на стороне клиента отличается от версии, у вас есть на сервере, SignalR часто имеет возможность адаптироваться к разницу.</span><span class="sxs-lookup"><span data-stu-id="8657d-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="8657d-155">Например сервере под управлением SignalR версии 2 будет поддерживать клиентов, имеющих установлен 1.1.x, а также клиенты, имеющие версии 2 установлен.</span><span class="sxs-lookup"><span data-stu-id="8657d-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="8657d-156">Если разница между версию на сервере и на клиентском компьютере слишком велико, или если клиент является более новой версии сервера, SignalR выдает `InvalidOperationException` исключение, когда клиент пытается установить соединение.</span><span class="sxs-lookup"><span data-stu-id="8657d-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="8657d-157">Сообщение об ошибке "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`«.</span><span class="sxs-lookup"><span data-stu-id="8657d-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="8657d-158">Как установить соединение</span><span class="sxs-lookup"><span data-stu-id="8657d-158">How to establish a connection</span></span>

<span data-ttu-id="8657d-159">Чтобы установить соединение, то необходимо создать `HubConnection` объект и создать учетную запись-посредник.</span><span class="sxs-lookup"><span data-stu-id="8657d-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="8657d-160">Чтобы установить подключение, вызов `Start` метод `HubConnection` объекта.</span><span class="sxs-lookup"><span data-stu-id="8657d-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="8657d-161">Для клиентов JavaScript, вам необходимо зарегистрировать по крайней мере один обработчик событий, перед вызовом `Start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="8657d-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="8657d-162">Это не является обязательным для клиентов .NET.</span><span class="sxs-lookup"><span data-stu-id="8657d-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="8657d-163">Для клиентов JavaScript, созданный код прокси автоматически создает прокси-серверы для всех концентраторов, которые существуют на сервере, и регистрация обработчика — указывает, какие центры клиент планирует использовать.</span><span class="sxs-lookup"><span data-stu-id="8657d-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="8657d-164">Но для клиента .NET создать прокси-серверы центра вручную, поэтому SignalR предполагается, что вы будете использовать любой центр, создать прокси для.</span><span class="sxs-lookup"><span data-stu-id="8657d-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="8657d-165">В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR.</span><span class="sxs-lookup"><span data-stu-id="8657d-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="8657d-166">Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="8657d-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="8657d-167">`Start` Метод выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="8657d-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="8657d-168">Чтобы убедиться, что последующие строки кода, которые не выполняются до после установления соединения, используйте `await` в асинхронном методе ASP.NET 4.5 или `.Wait()` в синхронный метод.</span><span class="sxs-lookup"><span data-stu-id="8657d-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="8657d-169">Не используйте `.Wait()` в клиенте WinRT.</span><span class="sxs-lookup"><span data-stu-id="8657d-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="8657d-170">Междоменные подключения от клиентов Silverlight</span><span class="sxs-lookup"><span data-stu-id="8657d-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="8657d-171">Сведения о включении междоменные подключения от клиентов Silverlight, см. в разделе [внесения службы через границы домена](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="8657d-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="8657d-172">Настройка подключения</span><span class="sxs-lookup"><span data-stu-id="8657d-172">How to configure the connection</span></span>

<span data-ttu-id="8657d-173">Перед установкой соединения можно указать любой из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="8657d-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="8657d-174">Ограничение одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="8657d-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="8657d-175">Параметры строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8657d-175">Query string parameters.</span></span>
- <span data-ttu-id="8657d-176">Метод транспорта.</span><span class="sxs-lookup"><span data-stu-id="8657d-176">The transport method.</span></span>
- <span data-ttu-id="8657d-177">Заголовки HTTP.</span><span class="sxs-lookup"><span data-stu-id="8657d-177">HTTP headers.</span></span>
- <span data-ttu-id="8657d-178">Сертификаты клиента.</span><span class="sxs-lookup"><span data-stu-id="8657d-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="8657d-179">Как задать максимальное число одновременных подключений клиентов WPF</span><span class="sxs-lookup"><span data-stu-id="8657d-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="8657d-180">В WPF клиентов может потребоваться увеличить максимальное число одновременных подключений со значения по умолчанию 2.</span><span class="sxs-lookup"><span data-stu-id="8657d-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="8657d-181">Рекомендуемое значение — 10.</span><span class="sxs-lookup"><span data-stu-id="8657d-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="8657d-182">Дополнительные сведения см. в разделе [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="8657d-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="8657d-183">Как указать параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="8657d-183">How to specify query string parameters</span></span>

<span data-ttu-id="8657d-184">Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса на объект подключения.</span><span class="sxs-lookup"><span data-stu-id="8657d-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="8657d-185">В следующем примере показано, как параметра строки запроса в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="8657d-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="8657d-186">Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.</span><span class="sxs-lookup"><span data-stu-id="8657d-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="8657d-187">Как указать метод транспорта</span><span class="sxs-lookup"><span data-stu-id="8657d-187">How to specify the transport method</span></span>

<span data-ttu-id="8657d-188">В рамках процесса подключения клиента SignalR обычно согласовывает с сервером, чтобы определить лучший транспорт, который поддерживается с сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="8657d-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="8657d-189">Если вы уже знаете, какой транспорт, который вы хотите использовать, вы можете обойти этот процесс согласования.</span><span class="sxs-lookup"><span data-stu-id="8657d-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="8657d-190">Чтобы указать метод транспорта, передайте объект транспорта к методу Start.</span><span class="sxs-lookup"><span data-stu-id="8657d-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="8657d-191">Приведенный ниже показано, как указать метод транспорта в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="8657d-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="8657d-192">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) пространство имен включает следующие классы, которые можно использовать для указания транспортного уровня.</span><span class="sxs-lookup"><span data-stu-id="8657d-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="8657d-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="8657d-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="8657d-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="8657d-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="8657d-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (доступен только в том случае, когда сервер и клиент использовать .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="8657d-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="8657d-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (автоматически выбирает лучший транспорт, который поддерживается клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="8657d-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="8657d-197">Это транспорта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8657d-197">This is the default transport.</span></span> <span data-ttu-id="8657d-198">Передача ее в `Start` метод имеет тот же эффект, что не передавая никаких действий.)</span><span class="sxs-lookup"><span data-stu-id="8657d-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="8657d-199">ForeverFrame транспорта не включен в этот список, так как он используется только в браузерах.</span><span class="sxs-lookup"><span data-stu-id="8657d-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="8657d-200">Сведения о том, как проверить метод транспорта в серверном коде, см. в разделе [ASP.NET руководство по API концентраторов SignalR - сервер — как для получения сведений о клиенте из контекстного свойства](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="8657d-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="8657d-201">Дополнительные сведения о транспортах и в случае ошибки, см. в разделе [введение в SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="8657d-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="8657d-202">Как указать заголовки HTTP</span><span class="sxs-lookup"><span data-stu-id="8657d-202">How to specify HTTP headers</span></span>

<span data-ttu-id="8657d-203">Чтобы задать заголовки HTTP, используйте `Headers` свойство для объекта connection.</span><span class="sxs-lookup"><span data-stu-id="8657d-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="8657d-204">Приведенный ниже показано, как добавить заголовок HTTP.</span><span class="sxs-lookup"><span data-stu-id="8657d-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="8657d-205">Практическое задание сертификатов клиентов</span><span class="sxs-lookup"><span data-stu-id="8657d-205">How to specify client certificates</span></span>

<span data-ttu-id="8657d-206">Чтобы добавить сертификаты клиента, используйте `AddClientCertificate` метод для объекта connection.</span><span class="sxs-lookup"><span data-stu-id="8657d-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="8657d-207">Как создать прокси-сервера концентратора</span><span class="sxs-lookup"><span data-stu-id="8657d-207">How to create the Hub proxy</span></span>

<span data-ttu-id="8657d-208">Чтобы определить методы на клиенте, который можно вызвать концентратор с сервера, а также вызывать методы концентратора на сервере, создать прокси-сервер для центра, вызвав `CreateHubProxy` для объекта connection.</span><span class="sxs-lookup"><span data-stu-id="8657d-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="8657d-209">Строка передается в `CreateHubProxy` — это имя класса концентратора или имя, указанное в `HubName` атрибут, если план, который использовался на сервере.</span><span class="sxs-lookup"><span data-stu-id="8657d-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="8657d-210">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="8657d-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="8657d-211">**Класс концентратора на сервере**</span><span class="sxs-lookup"><span data-stu-id="8657d-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="8657d-212">**Создайте клиентский прокси для класса концентратора**</span><span class="sxs-lookup"><span data-stu-id="8657d-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="8657d-213">Если вы устанавливаете центр класса с `HubName` атрибута, используйте его.</span><span class="sxs-lookup"><span data-stu-id="8657d-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="8657d-214">**Класс концентратора на сервере**</span><span class="sxs-lookup"><span data-stu-id="8657d-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="8657d-215">**Создайте клиентский прокси для класса концентратора**</span><span class="sxs-lookup"><span data-stu-id="8657d-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="8657d-216">При вызове метода `HubConnection.CreateHubProxy` несколько раз с тем же `hubName`, будут получены такие же кэшируются `IHubProxy` объекта.</span><span class="sxs-lookup"><span data-stu-id="8657d-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="8657d-217">Как определить методы на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="8657d-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="8657d-218">Чтобы определить метод, который сервер может обратиться, использовать прокси-сервер `On` метод, чтобы зарегистрировать обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="8657d-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="8657d-219">Совпадение имен метод не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="8657d-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="8657d-220">Например `Clients.All.UpdateStockPrice` будет выполняться на сервере `updateStockPrice`, `updatestockprice`, или `UpdateStockPrice` на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8657d-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="8657d-221">Разные клиентские платформы имеют различные требования к как написать код метода для обновления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="8657d-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="8657d-222">Примеры, приведенные предназначены для клиентов WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="8657d-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="8657d-223">WPF, Silverlight и примеры приложений консоли, указанные в [отдельном разделе этой статьи](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="8657d-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="8657d-224">Методы без параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-224">Methods without parameters</span></span>

<span data-ttu-id="8657d-225">Если метод обработка не имеет параметров, используйте перегрузку метода нестандартную `On` метод:</span><span class="sxs-lookup"><span data-stu-id="8657d-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="8657d-226">**Серверный код вызова клиентского метода без параметров**</span><span class="sxs-lookup"><span data-stu-id="8657d-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="8657d-227">**WinRT клиентский код для метода вызывается с сервера без параметров ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="8657d-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="8657d-228">Методы с параметрами, указав типы параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="8657d-229">Если метод обработка имеет параметры, укажите типы параметров, как универсальные типы `On` метод.</span><span class="sxs-lookup"><span data-stu-id="8657d-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="8657d-230">Существуют универсальные перегрузки `On` метод, чтобы можно было указать параметры до 8 (4 на Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="8657d-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="8657d-231">В следующем примере передается один параметр `UpdateStockPrice` метод.</span><span class="sxs-lookup"><span data-stu-id="8657d-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="8657d-232">**Серверный код вызова клиентского метода с параметром**</span><span class="sxs-lookup"><span data-stu-id="8657d-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="8657d-233">**Класс Stock, используемое для параметра**</span><span class="sxs-lookup"><span data-stu-id="8657d-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="8657d-234">**WinRT клиентский код для метода вызывается с сервера с параметром ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="8657d-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="8657d-235">Методы с параметрами, указав динамические объекты для параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="8657d-236">В качестве альтернативы для задания параметров, универсальных типов `On` метод, параметры можно указать в качестве динамических объектов:</span><span class="sxs-lookup"><span data-stu-id="8657d-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="8657d-237">**Серверный код вызова клиентского метода с параметром**</span><span class="sxs-lookup"><span data-stu-id="8657d-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="8657d-238">**Класс Stock, используемое для параметра**</span><span class="sxs-lookup"><span data-stu-id="8657d-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="8657d-239">**WinRT клиентский код для метода вызывается с сервера с параметром, с помощью динамического объекта для параметра ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="8657d-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="8657d-240">Как удалить обработчик</span><span class="sxs-lookup"><span data-stu-id="8657d-240">How to remove a handler</span></span>

<span data-ttu-id="8657d-241">Чтобы удалить обработчик, вызовите его `Dispose` метод.</span><span class="sxs-lookup"><span data-stu-id="8657d-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="8657d-242">**Клиентский код для метод, вызываемый из сервера**</span><span class="sxs-lookup"><span data-stu-id="8657d-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="8657d-243">**Клиентский код для удаления обработчика**</span><span class="sxs-lookup"><span data-stu-id="8657d-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="8657d-244">Как вызывать методы сервера от клиента</span><span class="sxs-lookup"><span data-stu-id="8657d-244">How to call server methods from the client</span></span>

<span data-ttu-id="8657d-245">Чтобы вызвать метод на сервере, используйте `Invoke` метод на прокси-сервера концентратора.</span><span class="sxs-lookup"><span data-stu-id="8657d-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="8657d-246">Если сервер метод не возвращает никакого значения, воспользуйтесь перегрузкой неуниверсальных `Invoke` метод.</span><span class="sxs-lookup"><span data-stu-id="8657d-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="8657d-247">**Серверный код для метода, который не имеет возвращаемого значения**</span><span class="sxs-lookup"><span data-stu-id="8657d-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="8657d-248">**Код клиента, вызов метода, который не имеет возвращаемого значения**</span><span class="sxs-lookup"><span data-stu-id="8657d-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="8657d-249">Если метод сервера имеет возвращаемое значение, укажите тип возвращаемого значения как универсальный тип `Invoke` метод.</span><span class="sxs-lookup"><span data-stu-id="8657d-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="8657d-250">**Серверный код для метода, который имеет возвращаемое значение и принимает параметр сложного типа**</span><span class="sxs-lookup"><span data-stu-id="8657d-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="8657d-251">**Класс Stock, используемый для параметра и возвращаемого значения**</span><span class="sxs-lookup"><span data-stu-id="8657d-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="8657d-252">**Клиентский код, вызвав метод, который имеет возвращаемое значение и принимает параметр сложного типа, в асинхронном методе ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="8657d-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="8657d-253">**Клиентский код, вызвав метод, который имеет возвращаемое значение и принимает параметр сложного типа, синхронный метод**</span><span class="sxs-lookup"><span data-stu-id="8657d-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="8657d-254">`Invoke` Метод выполняется асинхронно и возвращает `Task` объекта.</span><span class="sxs-lookup"><span data-stu-id="8657d-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="8657d-255">Если вы не укажете `await` или `.Wait()`, следующая строка кода будет выполняться до завершения выполнения метода, который вы вызываете.</span><span class="sxs-lookup"><span data-stu-id="8657d-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="8657d-256">Способ обработки событий времени существования подключений</span><span class="sxs-lookup"><span data-stu-id="8657d-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="8657d-257">SignalR обеспечивает следующее подключение события времени жизни, которые можно обработать:</span><span class="sxs-lookup"><span data-stu-id="8657d-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="8657d-258">`Received`: Вызывается, когда все данные получаются через соединение.</span><span class="sxs-lookup"><span data-stu-id="8657d-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="8657d-259">Предоставляет полученных данных.</span><span class="sxs-lookup"><span data-stu-id="8657d-259">Provides the received data.</span></span>
- <span data-ttu-id="8657d-260">`ConnectionSlow`: Вызывается, когда клиент обнаруживает подключение медленно или часто удаление.</span><span class="sxs-lookup"><span data-stu-id="8657d-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="8657d-261">`Reconnecting`: Вызывается, когда используемому транспорту начинает повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="8657d-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="8657d-262">`Reconnected`: Вызывается, когда была повторно присоединена используемому транспорту.</span><span class="sxs-lookup"><span data-stu-id="8657d-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="8657d-263">`StateChanged`: Возникает при изменении состояния подключения.</span><span class="sxs-lookup"><span data-stu-id="8657d-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="8657d-264">Предоставляет состояние старое и новое состояние.</span><span class="sxs-lookup"><span data-stu-id="8657d-264">Provides the old state and the new state.</span></span> <span data-ttu-id="8657d-265">Сведения о подключении, значения состояния см. в разделе [перечисление ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="8657d-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="8657d-266">`Closed`: Вызывается, когда произойдет отключение соединения.</span><span class="sxs-lookup"><span data-stu-id="8657d-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="8657d-267">Например, если вы хотите отображать предупреждающие сообщения для ошибок, которые не являются очень существенными, но вызывают проблемы с промежуточными соединениями, такие как медленная работа или часто удаление соединения, обрабатывать `ConnectionSlow` событий.</span><span class="sxs-lookup"><span data-stu-id="8657d-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="8657d-268">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="8657d-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="8657d-269">Способ обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="8657d-269">How to handle errors</span></span>

<span data-ttu-id="8657d-270">Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке.</span><span class="sxs-lookup"><span data-stu-id="8657d-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="8657d-271">Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Отправка подробных сообщений об ошибках клиентов в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.</span><span class="sxs-lookup"><span data-stu-id="8657d-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="8657d-272">Для обработки ошибок, которые вызывает SignalR, можно добавить обработчик для `Error` событие для объекта подключения.</span><span class="sxs-lookup"><span data-stu-id="8657d-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="8657d-273">Для обработки ошибок из вызовов методов, поместите код в блок try-catch.</span><span class="sxs-lookup"><span data-stu-id="8657d-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="8657d-274">Как включить ведение журнала на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="8657d-274">How to enable client-side logging</span></span>

<span data-ttu-id="8657d-275">Чтобы включить ведение журнала на стороне клиента, задайте `TraceLevel` и `TraceWriter` свойства в объекте подключения.</span><span class="sxs-lookup"><span data-stu-id="8657d-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="8657d-276">WPF, Silverlight и образцов кода консольных приложений для клиентских методов, которые могут вызывать сервера</span><span class="sxs-lookup"><span data-stu-id="8657d-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="8657d-277">Примеры кода, показанный ранее, для определения методов клиента, которые могут вызывать сервера применяются к клиентам WinRT.</span><span class="sxs-lookup"><span data-stu-id="8657d-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="8657d-278">В следующих примерах показано эквивалентный код для WPF, Silverlight и консоли приложений-клиентов.</span><span class="sxs-lookup"><span data-stu-id="8657d-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="8657d-279">Методы без параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-279">Methods without parameters</span></span>

<span data-ttu-id="8657d-280">**WPF клиентский код для метода с сервера без параметров**</span><span class="sxs-lookup"><span data-stu-id="8657d-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="8657d-281">**Код клиента Silverlight для метод, вызываемый с сервера без параметров**</span><span class="sxs-lookup"><span data-stu-id="8657d-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="8657d-282">**Код клиента приложения консоли для метода вызывается с сервера без параметров**</span><span class="sxs-lookup"><span data-stu-id="8657d-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="8657d-283">Методы с параметрами, указав типы параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="8657d-284">**Код клиента WPF для метод, вызываемый из сервера с параметром**</span><span class="sxs-lookup"><span data-stu-id="8657d-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="8657d-285">**Код клиента Silverlight для метод, вызываемый из сервера с параметром**</span><span class="sxs-lookup"><span data-stu-id="8657d-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="8657d-286">**Код клиента приложения консоли для метода вызывается с сервера с параметром**</span><span class="sxs-lookup"><span data-stu-id="8657d-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="8657d-287">Методы с параметрами, указав динамические объекты для параметров</span><span class="sxs-lookup"><span data-stu-id="8657d-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="8657d-288">**Код клиента WPF для метод, вызываемый из сервера с параметром, с помощью динамического объекта для параметра**</span><span class="sxs-lookup"><span data-stu-id="8657d-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="8657d-289">**Код клиента Silverlight для метод, вызываемый из сервера с параметром, с помощью динамического объекта для параметра**</span><span class="sxs-lookup"><span data-stu-id="8657d-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="8657d-290">**Код клиента приложения консоли для метода вызывается с сервера с параметром, с помощью динамического объекта для параметра**</span><span class="sxs-lookup"><span data-stu-id="8657d-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
