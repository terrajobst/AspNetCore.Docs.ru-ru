---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Руководство по API концентраторов ASP.NET SignalR - сервера (SignalR 1.x) | Документация Майкрософт
author: pfletcher
description: Этот документ содержит общие сведения о программировании серверной части API концентраторов SignalR ASP.NET для SignalR версии 1.1, с помощью demonstratin образцы кода...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 5597b89688382ab7eb2c2c724baf9a32abee49c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396670"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="72cca-103">Руководство по API концентраторов ASP.NET SignalR - сервера (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="72cca-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="72cca-104">по [Флетчера Патрик](https://github.com/pfletcher), [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="72cca-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="72cca-105">Этот документ содержит общие сведения о программировании серверной части API концентраторов SignalR ASP.NET для версии 1.1, SignalR с образцы кода, демонстрирующие общих параметров.</span><span class="sxs-lookup"><span data-stu-id="72cca-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="72cca-106">API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу.</span><span class="sxs-lookup"><span data-stu-id="72cca-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="72cca-107">В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="72cca-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="72cca-108">В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере.</span><span class="sxs-lookup"><span data-stu-id="72cca-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="72cca-109">SignalR берет на себя все необходимое для вас клиент сервер.</span><span class="sxs-lookup"><span data-stu-id="72cca-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="72cca-110">SignalR также предлагает API низкого уровня, вызывается постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="72cca-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="72cca-111">Введение в SignalR, концентраторы и постоянные подключения, или в этом учебнике показано, как создать полное приложение SignalR, см. в разделе [SignalR — Приступая к работе](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="72cca-112">Обзор</span><span class="sxs-lookup"><span data-stu-id="72cca-112">Overview</span></span>

<span data-ttu-id="72cca-113">Этот документ содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="72cca-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="72cca-114">Регистрация маршрутов SignalR и настройте параметры SignalR</span><span class="sxs-lookup"><span data-stu-id="72cca-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="72cca-115">URL-адрес /signalr</span><span class="sxs-lookup"><span data-stu-id="72cca-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="72cca-116">Настройка параметров SignalR</span><span class="sxs-lookup"><span data-stu-id="72cca-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="72cca-117">Как создать и использовать классы концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="72cca-118">Время жизни объекта концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="72cca-119">Венгерской имена центров в клиентах JavaScript</span><span class="sxs-lookup"><span data-stu-id="72cca-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="72cca-120">Несколько концентраторов</span><span class="sxs-lookup"><span data-stu-id="72cca-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="72cca-121">Как определить методы в классе концентратора, который клиенты могут вызывать</span><span class="sxs-lookup"><span data-stu-id="72cca-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="72cca-122">Венгерской имена методов в клиентах JavaScript</span><span class="sxs-lookup"><span data-stu-id="72cca-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="72cca-123">Когда следует выполнить асинхронно</span><span class="sxs-lookup"><span data-stu-id="72cca-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="72cca-124">Определение перегрузки</span><span class="sxs-lookup"><span data-stu-id="72cca-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="72cca-125">Порядок вызова методов клиента от концентратора класса</span><span class="sxs-lookup"><span data-stu-id="72cca-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="72cca-126">Выбор клиентов, которые будут получать RPC</span><span class="sxs-lookup"><span data-stu-id="72cca-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="72cca-127">Имена методов не проверяются во время компиляции</span><span class="sxs-lookup"><span data-stu-id="72cca-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="72cca-128">Совпадение имен метода, без учета регистра</span><span class="sxs-lookup"><span data-stu-id="72cca-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="72cca-129">Асинхронное выполнение</span><span class="sxs-lookup"><span data-stu-id="72cca-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="72cca-130">Как управлять членством в группах от класса концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="72cca-131">Асинхронное выполнение методов Add и Remove</span><span class="sxs-lookup"><span data-stu-id="72cca-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="72cca-132">Сохраняемость членство группы</span><span class="sxs-lookup"><span data-stu-id="72cca-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="72cca-133">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="72cca-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="72cca-134">Способ обработки событий времени существования подключений в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="72cca-135">При вызове OnConnected OnDisconnected и OnReconnected</span><span class="sxs-lookup"><span data-stu-id="72cca-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="72cca-136">Состояние вызывающего объекта не заполнен</span><span class="sxs-lookup"><span data-stu-id="72cca-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="72cca-137">Как получить сведения о клиенте из контекстного свойства</span><span class="sxs-lookup"><span data-stu-id="72cca-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="72cca-138">Способ передачи состояния между клиентами и классу Hub</span><span class="sxs-lookup"><span data-stu-id="72cca-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="72cca-139">Способ обработки ошибок в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="72cca-140">Как вызывать методы клиента и управлять ими групп за пределами классу Hub</span><span class="sxs-lookup"><span data-stu-id="72cca-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="72cca-141">Вызов методов клиента</span><span class="sxs-lookup"><span data-stu-id="72cca-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="72cca-142">Управление членством в группе</span><span class="sxs-lookup"><span data-stu-id="72cca-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="72cca-143">Включение трассировки</span><span class="sxs-lookup"><span data-stu-id="72cca-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="72cca-144">Настройка конвейера концентраторов</span><span class="sxs-lookup"><span data-stu-id="72cca-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="72cca-145">Документацию о том, как клиенты программы см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="72cca-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="72cca-146">Руководство по API концентраторов SignalR — клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="72cca-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="72cca-147">Руководство по API концентраторов SignalR — клиент .NET</span><span class="sxs-lookup"><span data-stu-id="72cca-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="72cca-148">Приведены ссылки на разделы, справочник по API .NET 4.5 версия API.</span><span class="sxs-lookup"><span data-stu-id="72cca-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="72cca-149">Если вы используете .NET 4, см. в разделе [версия .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="72cca-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="72cca-150">Регистрация маршрутов SignalR и настройте параметры SignalR</span><span class="sxs-lookup"><span data-stu-id="72cca-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="72cca-151">Чтобы определить маршрут, который будет использоваться клиентами для подключения к центру, вызовите [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) метод при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="72cca-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="72cca-152">`MapHubs` — [метод расширения](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) для `System.Web.Routing.RouteCollection` класса.</span><span class="sxs-lookup"><span data-stu-id="72cca-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="72cca-153">В следующем примере показано, как для определения маршрута концентраторы SignalR в *Global.asax* файл.</span><span class="sxs-lookup"><span data-stu-id="72cca-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="72cca-154">Функциональные возможности SignalR при добавлении в приложение ASP.NET MVC, убедитесь, что раньше, чем другие маршруты добавляется маршрут SignalR.</span><span class="sxs-lookup"><span data-stu-id="72cca-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="72cca-155">Дополнительные сведения см. в разделе [учебник: начало работы с SignalR и MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="72cca-156">URL-адрес /signalr</span><span class="sxs-lookup"><span data-stu-id="72cca-156">The /signalr URL</span></span>

<span data-ttu-id="72cca-157">По умолчанию является URL-адрес маршрута, который клиенты будут использовать для подключения к центру «/ signalr».</span><span class="sxs-lookup"><span data-stu-id="72cca-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="72cca-158">(Не путайте этот URL-адрес с URL-адрес «/ signalr/концентраторы», который является для автоматически создаваемого файла JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="72cca-159">Дополнительные сведения о созданном прокси, см. в разделе [руководство по API концентраторов SignalR — клиент JavaScript - созданный прокси и что он делает для вас](index.md).)</span><span class="sxs-lookup"><span data-stu-id="72cca-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="72cca-160">Могут возникнуть непредвиденные обстоятельства, которые делают этот базовый URL-адрес не может использоваться для SignalR; Например, у вас есть папка в проекте с именем *signalr* и вы не хотите изменить имя.</span><span class="sxs-lookup"><span data-stu-id="72cca-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="72cca-161">В этом случае можно изменить базовый URL-адрес, как показано в следующих примерах (Замените «/ signalr» в образце кода с нужный URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="72cca-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="72cca-162">**Код сервера, который указывает URL-адрес**</span><span class="sxs-lookup"><span data-stu-id="72cca-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="72cca-163">**Клиентский код JavaScript, который указывает URL-адрес (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="72cca-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="72cca-164">**Клиентский код JavaScript, который указывает URL-адрес (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="72cca-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="72cca-165">**Код клиента .NET, который указывает URL-адрес**</span><span class="sxs-lookup"><span data-stu-id="72cca-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="72cca-166">Настройка параметров SignalR</span><span class="sxs-lookup"><span data-stu-id="72cca-166">Configuring SignalR Options</span></span>

<span data-ttu-id="72cca-167">Перегруженные версии `MapHubs` метод дают возможность задать пользовательский URL-адрес сопоставителя пользовательскую зависимость и следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="72cca-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="72cca-168">Разрешить междоменные вызовы из клиентского обозревателя.</span><span class="sxs-lookup"><span data-stu-id="72cca-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="72cca-169">Обычно если браузер загружает страницу из `http://contoso.com`, подключении SignalR находится в том же домене, в `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="72cca-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="72cca-170">Если страницы от `http://contoso.com` подключается к `http://fabrikam.com/signalr`, то есть соединение между доменами.</span><span class="sxs-lookup"><span data-stu-id="72cca-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="72cca-171">По соображениям безопасности подключений между доменами отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="72cca-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="72cca-172">Дополнительные сведения см. в разделе [ASP.NET руководство по API концентраторов SignalR — клиент JavaScript - как для установления соединения между доменами](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="72cca-173">Включите подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="72cca-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="72cca-174">При возникновении ошибок, для отправки клиентам сообщение уведомления без подробной информации о произошедшее является поведение по умолчанию SignalR.</span><span class="sxs-lookup"><span data-stu-id="72cca-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="72cca-175">Отправки клиентам подробные сведения об ошибке в рабочей среде, не рекомендуется, поскольку пользователи-злоумышленники, можно использовать информацию в атаки, направленные на приложение.</span><span class="sxs-lookup"><span data-stu-id="72cca-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="72cca-176">Для устранения неполадок, можно использовать этот параметр временно включить более информативные отчеты об ошибках.</span><span class="sxs-lookup"><span data-stu-id="72cca-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="72cca-177">Отключите автоматически созданные файлы прокси JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="72cca-178">По умолчанию файл JavaScript с прокси-серверы для классов концентратор создается в ответ на URL-адрес «/ signalr/концентраторы».</span><span class="sxs-lookup"><span data-stu-id="72cca-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="72cca-179">Если вы не хотите использовать прокси-серверы JavaScript, или если вы хотите создать этот файл вручную и ссылаться на физическом файле в ваших клиентов, можно использовать этот параметр, чтобы отключить создание прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="72cca-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="72cca-180">Дополнительные сведения см. в разделе [руководство по API концентраторов SignalR — клиент JavaScript - прокси, созданного как создать физический файл для SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="72cca-181">Приведенный ниже показано, как указать URL-адрес подключения SignalR и эти параметры в вызове `MapHubs` метод.</span><span class="sxs-lookup"><span data-stu-id="72cca-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="72cca-182">Чтобы задать пользовательский URL-адрес, замените «/ signalr» в примере с URL-адрес, который вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="72cca-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="72cca-183">Как создать и использовать классы концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-183">How to create and use Hub classes</span></span>

<span data-ttu-id="72cca-184">Чтобы создать концентратор, создайте класс, производный от [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="72cca-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="72cca-185">В следующем примере показан простой класс концентратора для приложения разговора.</span><span class="sxs-lookup"><span data-stu-id="72cca-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="72cca-186">В этом примере подключенный клиент может вызвать `NewContosoChatMessage` метод, и когда это происходит, данные, полученные широковещательная рассылка на всех подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="72cca-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="72cca-187">Время жизни объекта концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-187">Hub object lifetime</span></span>

<span data-ttu-id="72cca-188">Не создавать экземпляр класса концентратора или вызывать его методы из собственного кода на сервере; все это выполняется автоматически конвейером концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="72cca-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="72cca-189">SignalR создает новый экземпляр класса концентратора каждый раз, когда оно будет обрабатывать операции концентратора, например когда клиент подключается, отключается или выполняет вызов метода к серверу.</span><span class="sxs-lookup"><span data-stu-id="72cca-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="72cca-190">Так как экземпляры класса концентратора являются временными, их нельзя использовать для поддержания состояния от одного вызова метода к другому.</span><span class="sxs-lookup"><span data-stu-id="72cca-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="72cca-191">Каждый раз, сервер получает вызов метода из клиента, новый экземпляр класса процессов центра сообщения.</span><span class="sxs-lookup"><span data-stu-id="72cca-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="72cca-192">Для поддержания состояния, через несколько подключений и вызовы методов, использовать другой метод, например базы данных или статической переменной на класс концентратора или другого класса, который является производным от `Hub`.</span><span class="sxs-lookup"><span data-stu-id="72cca-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="72cca-193">Если сохранить данные в памяти, с помощью метода, например в статической переменной класса концентратора, данные будут потеряны при очистке домена приложения.</span><span class="sxs-lookup"><span data-stu-id="72cca-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="72cca-194">Если вы хотите отправлять сообщения на клиентах из собственного кода, запускаемый за пределами классу Hub, это нельзя сделать, создание экземпляра класса концентратора, но это можно сделать, получив ссылку на объект контекста SignalR для класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="72cca-195">Дополнительные сведения см. в разделе [как вызывать методы клиента и управлять ими групп за пределами классу Hub](#callfromoutsidehub) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="72cca-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="72cca-196">Венгерской имена центров в клиентах JavaScript</span><span class="sxs-lookup"><span data-stu-id="72cca-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="72cca-197">По умолчанию клиенты JavaScript ссылаются концентраторов с использованием версии стиле Camel имени класса.</span><span class="sxs-lookup"><span data-stu-id="72cca-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="72cca-198">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="72cca-199">Предыдущий пример будет называться `contosoChatHub` в коде JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="72cca-200">**Сервер**</span><span class="sxs-lookup"><span data-stu-id="72cca-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="72cca-201">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="72cca-202">Если вы хотите указать другое имя для клиентов для использования, добавьте `HubName` атрибута.</span><span class="sxs-lookup"><span data-stu-id="72cca-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="72cca-203">При использовании `HubName` атрибут, никак не изменяется имя в стиль Camel в клиентах JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="72cca-204">**Сервер**</span><span class="sxs-lookup"><span data-stu-id="72cca-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="72cca-205">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="72cca-206">Несколько концентраторов</span><span class="sxs-lookup"><span data-stu-id="72cca-206">Multiple Hubs</span></span>

<span data-ttu-id="72cca-207">Можно определить несколько классов концентратора в приложении.</span><span class="sxs-lookup"><span data-stu-id="72cca-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="72cca-208">При этом общий доступ к подключению, но отделены групп:</span><span class="sxs-lookup"><span data-stu-id="72cca-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="72cca-209">Все клиенты будут использовать же URL-адрес для подключения SignalR с вашей службой («/ signalr» или пользовательский URL-адрес, если вы ее указали), и что соединение используется для всех концентраторов, определенной службой.</span><span class="sxs-lookup"><span data-stu-id="72cca-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="72cca-210">Нет никакой разницы производительности для нескольких центров, по сравнению с определение все функциональные возможности центра в одном классе.</span><span class="sxs-lookup"><span data-stu-id="72cca-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="72cca-211">Все центры получить те же сведения запроса HTTP.</span><span class="sxs-lookup"><span data-stu-id="72cca-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="72cca-212">Так как все концентраторы используют то же подключение, только сведения о запросе HTTP, сервер возвращает становится поставляются в исходном запросе HTTP, который устанавливает соединение SignalR.</span><span class="sxs-lookup"><span data-stu-id="72cca-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="72cca-213">Если запрос на подключение используется для передачи данных из клиента на сервер, указав строку запроса, не может предоставить различные строки запроса для разных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="72cca-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="72cca-214">Все концентраторы будут получать те же сведения.</span><span class="sxs-lookup"><span data-stu-id="72cca-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="72cca-215">Созданный файл прокси JavaScript будет содержать учетные записи-посредники для всех концентраторов в одном файле.</span><span class="sxs-lookup"><span data-stu-id="72cca-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="72cca-216">Сведения о прокси JavaScript, см. в разделе [руководство по API концентраторов SignalR — клиент JavaScript - созданный прокси и что он делает для вас](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="72cca-217">Группы определяются в пределах концентраторов.</span><span class="sxs-lookup"><span data-stu-id="72cca-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="72cca-218">В SignalR, которые можно определить именованные группы, чтобы вещать на подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="72cca-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="72cca-219">Группы поддерживаются отдельно для каждого центра.</span><span class="sxs-lookup"><span data-stu-id="72cca-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="72cca-220">Например, группу с именем «Администраторы» будет включать в себя набор клиентов для вашего `ContosoChatHub` класс и имя группы ссылаетесь на другой набор клиентов для вашего `StockTickerHub` класса.</span><span class="sxs-lookup"><span data-stu-id="72cca-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="72cca-221">Как определить методы в классе концентратора, который клиенты могут вызывать</span><span class="sxs-lookup"><span data-stu-id="72cca-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="72cca-222">Чтобы предоставить метод на концентраторе, который будет вызываться из клиента, объявите открытый метод, как показано в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="72cca-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="72cca-223">Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#.</span><span class="sxs-lookup"><span data-stu-id="72cca-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="72cca-224">Любые данные, которые вы получаете в параметрах или возвращать вызывающей стороне передается между клиентом и сервером с помощью JSON и SignalR обрабатывает привязки сложных объектов и массивов объектов автоматически.</span><span class="sxs-lookup"><span data-stu-id="72cca-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="72cca-225">Венгерской имена методов в клиентах JavaScript</span><span class="sxs-lookup"><span data-stu-id="72cca-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="72cca-226">По умолчанию клиенты JavaScript ссылаются методов концентратора с помощью версии стиле Camel имени метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="72cca-227">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="72cca-228">**Сервер**</span><span class="sxs-lookup"><span data-stu-id="72cca-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="72cca-229">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="72cca-230">Если вы хотите указать другое имя для клиентов для использования, добавьте `HubMethodName` атрибута.</span><span class="sxs-lookup"><span data-stu-id="72cca-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="72cca-231">**Сервер**</span><span class="sxs-lookup"><span data-stu-id="72cca-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="72cca-232">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="72cca-233">Когда следует выполнить асинхронно</span><span class="sxs-lookup"><span data-stu-id="72cca-233">When to execute asynchronously</span></span>

<span data-ttu-id="72cca-234">Если метод будет иметь долго выполняющиеся или должен работать будет включать оповещения, такие как поиск в базе данных или вызов веб-службы, сделать асинхронного метода концентратора, возвращая [задачи](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (вместо `void` возврата) или [ Задача&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) объектов (вместо `T` тип возвращаемого значения).</span><span class="sxs-lookup"><span data-stu-id="72cca-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="72cca-235">При возврате `Task` ожидает объект из метода, SignalR `Task` для завершения, а затем отправляет без оболочки результат обратно клиенту, поэтому нет никакой разницы в как кода вызов метода в клиенте.</span><span class="sxs-lookup"><span data-stu-id="72cca-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="72cca-236">Что делает метод концентратора на асинхронных позволяет избежать блокирует подключения, когда используется транспорт WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72cca-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="72cca-237">Когда метод концентратора выполняется синхронно и транспортом является WebSocket, последующие вызовы методов концентратора от одного клиента блокируются до завершения метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="72cca-238">В следующем примере показано, тот же метод в коде выполняются синхронно или асинхронно, а затем клиентский код JavaScript, подходящий для вызова любой из версий.</span><span class="sxs-lookup"><span data-stu-id="72cca-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="72cca-239">**Синхронный**</span><span class="sxs-lookup"><span data-stu-id="72cca-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="72cca-240">**Асинхронные - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="72cca-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="72cca-241">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="72cca-242">Дополнительные сведения о способах использования асинхронных методов в ASP.NET 4.5 см. в разделе [использование асинхронных методов в ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="72cca-243">Определение перегрузки</span><span class="sxs-lookup"><span data-stu-id="72cca-243">Defining Overloads</span></span>

<span data-ttu-id="72cca-244">Если вы хотите определить перегрузок для метода, число параметров в каждой перегрузке должны быть разными.</span><span class="sxs-lookup"><span data-stu-id="72cca-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="72cca-245">Если отличить перегрузку, просто указав разные типы параметров, ваш класс концентратора скомпилируется, но служба SignalR приведет к возникновению исключения во время выполнения, когда клиенты пытаются для вызова одной из перегрузок.</span><span class="sxs-lookup"><span data-stu-id="72cca-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="72cca-246">Порядок вызова методов клиента от концентратора класса</span><span class="sxs-lookup"><span data-stu-id="72cca-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="72cca-247">Чтобы вызвать методы клиента с сервера, используйте `Clients` свойство в метод в классе концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="72cca-248">В следующем примере показано код сервера, который вызывает `addNewMessageToPage` на всех подключенных клиентах и код клиента, который определяет метод, в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="72cca-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="72cca-249">**Сервер**</span><span class="sxs-lookup"><span data-stu-id="72cca-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="72cca-250">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="72cca-251">Не удается получить значение, возвращаемое из метода клиента; синтаксис, такие как `int x = Clients.All.add(1,1)` не работает.</span><span class="sxs-lookup"><span data-stu-id="72cca-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="72cca-252">Можно указать сложные типы и массивы параметров.</span><span class="sxs-lookup"><span data-stu-id="72cca-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="72cca-253">В следующем примере передается сложный тип клиенту в параметре метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="72cca-254">**Код сервера, который вызывает клиентский метод, с помощью сложного объекта**</span><span class="sxs-lookup"><span data-stu-id="72cca-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="72cca-255">**Код сервера, который определяет сложный объект**</span><span class="sxs-lookup"><span data-stu-id="72cca-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="72cca-256">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="72cca-257">Выбор клиентов, которые будут получать RPC</span><span class="sxs-lookup"><span data-stu-id="72cca-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="72cca-258">Это свойство возвращает клиентов [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) объект, который предоставляет несколько вариантов для указания, какие клиенты будут получать RPC:</span><span class="sxs-lookup"><span data-stu-id="72cca-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="72cca-259">Все подключенные клиенты.</span><span class="sxs-lookup"><span data-stu-id="72cca-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="72cca-260">Вызывающий клиент.</span><span class="sxs-lookup"><span data-stu-id="72cca-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="72cca-261">Все клиенты, кроме вызывающего клиента.</span><span class="sxs-lookup"><span data-stu-id="72cca-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="72cca-262">Конкретного клиента, определяемого по идентификатору подключения.</span><span class="sxs-lookup"><span data-stu-id="72cca-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="72cca-263">В этом примере вызывается `addContosoChatMessageToPage` вызывающему клиенту и имеет тот же эффект, как с помощью `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="72cca-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="72cca-264">Все подключенные клиенты, кроме указанным клиентам, идентифицируемый идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="72cca-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="72cca-265">Все подключенные клиенты в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="72cca-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="72cca-266">Все подключенные клиенты в указанной группе, кроме указанным клиентам, идентифицируемый идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="72cca-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="72cca-267">Все подключенные клиенты в указанной группе, кроме вызывающего клиента.</span><span class="sxs-lookup"><span data-stu-id="72cca-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="72cca-268">Имена методов не проверяются во время компиляции</span><span class="sxs-lookup"><span data-stu-id="72cca-268">No compile-time validation for method names</span></span>

<span data-ttu-id="72cca-269">Имя метода, указать интерпретируется как динамический объект, это означает, что нет IntelliSense или проверку во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="72cca-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="72cca-270">Выражение вычисляется во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="72cca-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="72cca-271">При выполнении вызова метода, SignalR отправляет имя метода и значения параметров клиента, а если клиент имеет метод, совпадающий с именем, что метод вызывается и значения параметров передаются в него.</span><span class="sxs-lookup"><span data-stu-id="72cca-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="72cca-272">Если отсутствует соответствующий метод находится на стороне клиента, ошибка не возникает.</span><span class="sxs-lookup"><span data-stu-id="72cca-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="72cca-273">Сведения о формате данных, SignalR передает клиенту за кулисами при вызове метода клиента, см. в разделе [введение в SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="72cca-274">Совпадение имен метода, без учета регистра</span><span class="sxs-lookup"><span data-stu-id="72cca-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="72cca-275">Совпадение имен метод не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="72cca-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="72cca-276">Например `Clients.All.addContosoChatMessageToPage` будет выполняться на сервере `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, или `addContosoChatMessageToPage` на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="72cca-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="72cca-277">Асинхронное выполнение</span><span class="sxs-lookup"><span data-stu-id="72cca-277">Asynchronous execution</span></span>

<span data-ttu-id="72cca-278">Асинхронно выполняет метод, который вы вызываете.</span><span class="sxs-lookup"><span data-stu-id="72cca-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="72cca-279">Любой код, который после вызова метода клиенту начинается немедленно без ожидания SignalR для завершения передачи данных для клиентов в том случае, если не указано, в последующих строках кода следует подождать завершения метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="72cca-280">В следующих примерах кода показано, как для последовательного выполнения двух методов клиента, его с помощью кода, которое работает в .NET 4.5 и ее с помощью кода, которое работает в .NET 4.</span><span class="sxs-lookup"><span data-stu-id="72cca-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="72cca-281">**Пример .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="72cca-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="72cca-282">**Пример .NET 4**</span><span class="sxs-lookup"><span data-stu-id="72cca-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="72cca-283">Если вы используете `await` или `ContinueWith` подождать, пока клиентский метод завершения до выполнения следующей строке кода, это не означает, что клиенты фактически будут получать сообщение перед выполнением следующей строке кода.</span><span class="sxs-lookup"><span data-stu-id="72cca-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="72cca-284">«Дополнение» клиентского вызова метода только означает, что SignalR выполняться все компоненты, необходимые для отправки сообщения.</span><span class="sxs-lookup"><span data-stu-id="72cca-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="72cca-285">Если вам нужна проверка, что клиенты получили сообщение, вам нужно программировать этот механизм самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="72cca-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="72cca-286">Например, вы кода `MessageReceived` метод на концентраторе и в `addContosoChatMessageToPage` метод на стороне клиента, можно вызвать `MessageReceived` после выполнения работа вам нужно сделать на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="72cca-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="72cca-287">В `MessageReceived` концентратора можно делать любые зависят от фактического клиентского приема и обработки исходного вызова метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="72cca-288">Использование строковой переменной в качестве имени метода</span><span class="sxs-lookup"><span data-stu-id="72cca-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="72cca-289">Если вы хотите вызвать клиентский метод, используя строковую переменную в качестве имени метода, приведение `Clients.All` (или `Clients.Others`, `Clients.Caller`т. д.) для `IClientProxy` , а затем вызвать [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="72cca-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="72cca-290">Как управлять членством в группах от класса концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="72cca-291">Группами в SignalR предоставляют метод широковещательная рассылка сообщений для заданного подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="72cca-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="72cca-292">Группа может иметь любое число клиентов, и клиент может быть членом любое количество групп.</span><span class="sxs-lookup"><span data-stu-id="72cca-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="72cca-293">Чтобы управлять членством в группах, используйте [добавить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) и [удалить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) методы, предоставляемые `Groups` свойство класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="72cca-294">В следующем примере показан `Groups.Add` и `Groups.Remove` методы, используемые в методах Hub, которые вызываются в клиентском коде, следуют клиентского кода JavaScript, который вызывает их.</span><span class="sxs-lookup"><span data-stu-id="72cca-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="72cca-295">**Сервер**</span><span class="sxs-lookup"><span data-stu-id="72cca-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="72cca-296">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="72cca-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="72cca-297">Не нужно явно создавать группы.</span><span class="sxs-lookup"><span data-stu-id="72cca-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="72cca-298">Фактически группа автоматически создается первый раз, укажите его имя в вызове `Groups.Add`, и она будет удалена при удалении последнего соединения из членства в ней.</span><span class="sxs-lookup"><span data-stu-id="72cca-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="72cca-299">Не существует API для получения списка членства в группе или список групп.</span><span class="sxs-lookup"><span data-stu-id="72cca-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="72cca-300">SignalR отправляет сообщения клиентам и группам на основе [модели публикации и подписки](http://en.wikipedia.org/wiki/Publish/subscribe), и сервер не ведет список групп или членства в группах.</span><span class="sxs-lookup"><span data-stu-id="72cca-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="72cca-301">Это помогает добиться максимальной масштабируемости, так как каждый раз при добавлении узла к веб-ферме, любое состояние, которое поддерживает SignalR должен быть распространены на новый узел.</span><span class="sxs-lookup"><span data-stu-id="72cca-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="72cca-302">Асинхронное выполнение методов Add и Remove</span><span class="sxs-lookup"><span data-stu-id="72cca-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="72cca-303">`Groups.Add` И `Groups.Remove` методы асинхронного выполнения.</span><span class="sxs-lookup"><span data-stu-id="72cca-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="72cca-304">Если вы хотите добавить в клиент группу и немедленно отправить клиенту сообщение с помощью в группу, необходимо убедиться, что `Groups.Add` метод завершается первой.</span><span class="sxs-lookup"><span data-stu-id="72cca-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="72cca-305">В следующих примерах кода показано, как это сделать с помощью кода, который работает в .NET 4.5, а другой с помощью кода, который работает в .NET 4</span><span class="sxs-lookup"><span data-stu-id="72cca-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="72cca-306">**Пример .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="72cca-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="72cca-307">**Пример .NET 4**</span><span class="sxs-lookup"><span data-stu-id="72cca-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="72cca-308">Сохраняемость членство группы</span><span class="sxs-lookup"><span data-stu-id="72cca-308">Group membership persistence</span></span>

<span data-ttu-id="72cca-309">SignalR отслеживает подключений, которые не пользователей, поэтому, если пользователь должны находиться в той же группе каждый раз, когда пользователь выполняет подключение, должен быть вызван `Groups.Add` каждый раз, он устанавливает новое подключение.</span><span class="sxs-lookup"><span data-stu-id="72cca-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="72cca-310">После временной потери связи иногда SignalR можно восстановить подключение автоматически.</span><span class="sxs-lookup"><span data-stu-id="72cca-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="72cca-311">В этом случае SignalR восстановление одно и то же подключение, не создания нового подключения, и таким образом, членство в группе клиента восстанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="72cca-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="72cca-312">Это возможно даже в том случае, если временный разрыв не в результате перезагрузки сервера или сбой, так как состояние подключения для каждого клиента, включая членство в группах, обхода клиенту.</span><span class="sxs-lookup"><span data-stu-id="72cca-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="72cca-313">Если сервер выйдет из строя и заменяется на новый сервер до истечения времени ожидания соединения, клиент можно автоматически повторно подключиться к новому серверу и повторно зарегистрировать в группах, в которых он участвует.</span><span class="sxs-lookup"><span data-stu-id="72cca-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="72cca-314">Если соединение невозможно восстановить автоматически после потери подключения, или когда время ожидания соединения или при отключении клиента (например, когда браузер переходит на новую страницу), членства в группах, будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="72cca-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="72cca-315">При очередном подключении будет новое соединение.</span><span class="sxs-lookup"><span data-stu-id="72cca-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="72cca-316">Для обеспечения членства в группах при тот же пользователь устанавливает новое соединение, приложение должно отслеживать связи между пользователями и группами и восстановление членства в группах каждый раз, пользователь устанавливает новое соединение.</span><span class="sxs-lookup"><span data-stu-id="72cca-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="72cca-317">Дополнительные сведения о подключениях и переподключения см. в разделе [способ обработки событий времени существования подключений в классе концентратора](#connectionlifetime) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="72cca-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="72cca-318">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="72cca-318">Single-user groups</span></span>

<span data-ttu-id="72cca-319">Приложения, использующие SignalR обычно имеют для отслеживания сопоставлений пользователей и подключений, чтобы знать, какой пользователь отправил сообщение и какие пользователи должны получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="72cca-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="72cca-320">Группы используются в одном из двух часто используемых шаблонов для соответствующей.</span><span class="sxs-lookup"><span data-stu-id="72cca-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="72cca-321">Группы в однопользовательском режиме.</span><span class="sxs-lookup"><span data-stu-id="72cca-321">Single-user groups.</span></span>

    <span data-ttu-id="72cca-322">Можно указать имя пользователя в качестве имени группы и добавить идентификатор текущего подключения к группе, каждый раз пользователь подключается или повторном подключении.</span><span class="sxs-lookup"><span data-stu-id="72cca-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="72cca-323">Для отправки сообщений пользователю отправлять в группу.</span><span class="sxs-lookup"><span data-stu-id="72cca-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="72cca-324">Недостатком этого метода является то, что группе не предоставляет вам способ проверить, является ли пользователь интерактивном или автономном режиме.</span><span class="sxs-lookup"><span data-stu-id="72cca-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="72cca-325">Отслеживать связи между имена пользователей и идентификаторов подключений.</span><span class="sxs-lookup"><span data-stu-id="72cca-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="72cca-326">Можно хранить ассоциацию между каждого имени пользователя и один или несколько идентификаторов подключений в словарь или базы данных и обновлять сохраненные данные каждый раз, пользователь подключении или отключении.</span><span class="sxs-lookup"><span data-stu-id="72cca-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="72cca-327">Для отправки сообщений пользователю указать идентификаторов подключений.</span><span class="sxs-lookup"><span data-stu-id="72cca-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="72cca-328">Недостатком этого метода является то, что занимает больше памяти.</span><span class="sxs-lookup"><span data-stu-id="72cca-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="72cca-329">Способ обработки событий времени существования подключений в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="72cca-330">Типичной причиной для обработки события времени жизни соединения: для отслеживания ли пользователь подключен или не и для отслеживания сопоставление имен пользователей и идентификаторов подключений.</span><span class="sxs-lookup"><span data-stu-id="72cca-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="72cca-331">Для выполнения собственного кода, когда клиенты подключиться или отключиться, переопределите `OnConnected`, `OnDisconnected`, и `OnReconnected` класса виртуальных методов концентратора, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="72cca-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="72cca-332">При вызове OnConnected OnDisconnected и OnReconnected</span><span class="sxs-lookup"><span data-stu-id="72cca-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="72cca-333">Каждый раз, когда браузер переходит на новую страницу, новое соединение имеет установления, означающее, что будет выполняться SignalR `OnDisconnected` метода, за которым следует `OnConnected` метод.</span><span class="sxs-lookup"><span data-stu-id="72cca-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="72cca-334">SignalR всегда создает новый идентификатор соединения, если установлено новое соединение.</span><span class="sxs-lookup"><span data-stu-id="72cca-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="72cca-335">`OnReconnected` Метод вызывается в том случае, когда произошел временный разрыв подключения, можно автоматически восстановить SignalR, например когда кабель временно отключен и использовать подключение до истечения времени ожидания соединения. `OnDisconnected` Метод вызывается в том случае, когда клиент отключен и SignalR невозможностью автоматически, например когда браузер переходит на новую страницу.</span><span class="sxs-lookup"><span data-stu-id="72cca-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="72cca-336">Таким образом, возможно последовательность событий для данного клиента — `OnConnected`, `OnReconnected`, `OnDisconnected`; или `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="72cca-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="72cca-337">Вы не увидите последовательность `OnConnected`, `OnDisconnected`, `OnReconnected` для данного соединения.</span><span class="sxs-lookup"><span data-stu-id="72cca-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="72cca-338">`OnDisconnected` Метод не вызывается в некоторых сценариях, например если сервер выйдет из строя или домен приложения получает перезапущен.</span><span class="sxs-lookup"><span data-stu-id="72cca-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="72cca-339">Когда другой сервер переходит в оперативный или домена приложения по завершении его повторный запуск, некоторые клиенты могут иметь возможность повторного подключения и инициировать `OnReconnected` событий.</span><span class="sxs-lookup"><span data-stu-id="72cca-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="72cca-340">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="72cca-341">Состояние вызывающего объекта не заполнен</span><span class="sxs-lookup"><span data-stu-id="72cca-341">Caller state not populated</span></span>

<span data-ttu-id="72cca-342">Методы обработчиков событий времени существования соединения вызываются на сервере, это означает, что любое состояние, которое необходимо поместить в `state` объекта на стороне клиента не будут указаны в `Caller` свойство на сервере.</span><span class="sxs-lookup"><span data-stu-id="72cca-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="72cca-343">Сведения о `state` объекта и `Caller` свойство, см. в разделе [как для передачи состояния между клиентами и классу Hub](#passstate) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="72cca-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="72cca-344">Как получить сведения о клиенте из контекстного свойства</span><span class="sxs-lookup"><span data-stu-id="72cca-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="72cca-345">Чтобы получить сведения о клиенте, используйте `Context` свойство класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="72cca-346">`Context` Возвращает [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) объект, который предоставляет доступ к следующим сведениям:</span><span class="sxs-lookup"><span data-stu-id="72cca-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="72cca-347">Идентификатор подключения вызывающего клиента.</span><span class="sxs-lookup"><span data-stu-id="72cca-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="72cca-348">Идентификатор подключения — это GUID, назначенный SignalR (в собственном коде невозможно задать значение).</span><span class="sxs-lookup"><span data-stu-id="72cca-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="72cca-349">Есть один идентификатор подключения для каждого подключения и то же подключение, он используется во всех концентраторах, если у вас есть несколько концентраторов в приложении.</span><span class="sxs-lookup"><span data-stu-id="72cca-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="72cca-350">Данные заголовка HTTP.</span><span class="sxs-lookup"><span data-stu-id="72cca-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="72cca-351">Можно также получить заголовки HTTP из `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="72cca-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="72cca-352">Причина для нескольких ссылок на тот же том, что `Context.Headers` была создана, во-первых, `Context.Request` свойство было добавлено позже, и `Context.Headers` был сохранен для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="72cca-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="72cca-353">Запрос данных строки.</span><span class="sxs-lookup"><span data-stu-id="72cca-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="72cca-354">Можно также получить данные строки запроса из `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="72cca-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="72cca-355">Строка запроса, которую можно получить в этом свойстве является тот, который использовался с HTTP-запроса, установленного подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="72cca-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="72cca-356">Можно добавить параметры строки запроса в клиенте, настроив подключения, которая является удобным способом для передачи данных о клиенте от клиента к серверу.</span><span class="sxs-lookup"><span data-stu-id="72cca-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="72cca-357">В следующем примере показан один способ добавления строки запроса в клиенте JavaScript, при использовании созданного прокси.</span><span class="sxs-lookup"><span data-stu-id="72cca-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="72cca-358">Дополнительные сведения о настройке параметров строки запроса см. в разделе руководства по API для [JavaScript](index.md) и [.NET](index.md) клиентов.</span><span class="sxs-lookup"><span data-stu-id="72cca-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="72cca-359">Вы можете найти метод транспорта, используемый для соединения в данные строки запроса, а также некоторые другие значения, используемые внутри SignalR:</span><span class="sxs-lookup"><span data-stu-id="72cca-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="72cca-360">Значение `transportMethod` будет «webSockets», «serverSentEvents», «foreverFrame» или «longPolling».</span><span class="sxs-lookup"><span data-stu-id="72cca-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="72cca-361">Обратите внимание, что если вы установите это значение `OnConnected` метод обработчика событий, в некоторых сценариях может изначально получить значение транспорта, не является методом окончательного согласованного транспорта для подключения.</span><span class="sxs-lookup"><span data-stu-id="72cca-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="72cca-362">В этом случае метод вызовет исключение и вызывается позже при установлении метод окончательный транспорта.</span><span class="sxs-lookup"><span data-stu-id="72cca-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="72cca-363">Файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="72cca-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="72cca-364">Также можно получить файлы cookie из `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="72cca-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="72cca-365">Сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="72cca-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="72cca-366">Для запроса объекта HttpContext:</span><span class="sxs-lookup"><span data-stu-id="72cca-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="72cca-367">Используйте этот метод вместо получения `HttpContext.Current` для получения `HttpContext` объект для подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="72cca-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="72cca-368">Способ передачи состояния между клиентами и классу Hub</span><span class="sxs-lookup"><span data-stu-id="72cca-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="72cca-369">Предоставляет клиентский прокси `state` объекта, в котором можно хранить данные, которые могут передаваться на сервер с каждым вызовом метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="72cca-370">На сервере можно получить доступ к данным в `Clients.Caller` свойство в методах Hub, которые вызываются клиентами.</span><span class="sxs-lookup"><span data-stu-id="72cca-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="72cca-371">`Clients.Caller` Свойство не заполняется для методов обработчиков событий времени существования соединения `OnConnected`, `OnDisconnected`, и `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="72cca-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="72cca-372">Создание или обновление данных в `state` объекта и `Clients.Caller` свойство работает в обоих направлениях.</span><span class="sxs-lookup"><span data-stu-id="72cca-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="72cca-373">Можно обновить значения на сервере, и они передаются обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="72cca-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="72cca-374">В примере показан клиентский код JavaScript, который хранит состояние для передачи на сервер при каждом вызове метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="72cca-375">В следующем примере эквивалентный код в клиенте .NET.</span><span class="sxs-lookup"><span data-stu-id="72cca-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="72cca-376">В классе концентратора, можно получить доступ к эти данные в `Clients.Caller` свойство.</span><span class="sxs-lookup"><span data-stu-id="72cca-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="72cca-377">В примере показан код, который получает состояние, в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="72cca-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="72cca-378">Этот механизм для сохранения состояния не предназначен для больших объемов данных, так как все, что вы поместите в `state` или `Clients.Caller` свойства обхода с каждого вызова метода.</span><span class="sxs-lookup"><span data-stu-id="72cca-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="72cca-379">Это полезно для небольших элементов, таких как имена пользователей или счетчики.</span><span class="sxs-lookup"><span data-stu-id="72cca-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="72cca-380">Способ обработки ошибок в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="72cca-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="72cca-381">Для обработки ошибок, возникающих в методах класса концентратора, используйте один или оба из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="72cca-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="72cca-382">Перенос кода метода в блоки try-catch и войдите в объекте исключения.</span><span class="sxs-lookup"><span data-stu-id="72cca-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="72cca-383">Для целей отладки исключения можно отправить клиенту, но для обеспечения безопасности причин отправки подробные сведения клиентам в рабочей среде не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="72cca-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="72cca-384">Создание модуля концентраторов конвейера, который будет обрабатывать [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) метод.</span><span class="sxs-lookup"><span data-stu-id="72cca-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="72cca-385">В следующем примере показано модуль конвейера, который регистрирует ошибки, Далее следует код в файле Global.asax, который внедряет модуля в конвейере концентраторов.</span><span class="sxs-lookup"><span data-stu-id="72cca-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="72cca-386">Дополнительные сведения о модулях центра конвейера см. в разделе [способы настройки конвейера концентраторов](#hubpipeline) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="72cca-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="72cca-387">Включение трассировки</span><span class="sxs-lookup"><span data-stu-id="72cca-387">How to enable tracing</span></span>

<span data-ttu-id="72cca-388">Чтобы включить трассировку на стороне сервера, добавьте system.diagnostics-элемент в файл Web.config, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="72cca-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="72cca-389">При запуске приложения в Visual Studio, можно просмотреть журналы в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="72cca-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="72cca-390">Как вызывать методы клиента и управлять ими групп за пределами классу Hub</span><span class="sxs-lookup"><span data-stu-id="72cca-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="72cca-391">Для вызова методов клиента от другого класса, чем класс концентратора, получить ссылку на объект контекста SignalR для центра и использовать его для вызова методов на стороне клиента и управление группами.</span><span class="sxs-lookup"><span data-stu-id="72cca-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="72cca-392">Следующий пример `StockTicker` класса возвращает объект контекста, сохраняет его в экземпляр класса, хранит экземпляр класса в статическое свойство и использует контекст из одноэлементный экземпляр класса для вызова `updateStockPrice` метод на клиентах, которые являются подключение к концентратору с именем `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="72cca-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="72cca-393">Если вам нужно использовать время несколько контекста в долгоживущих объектов, получить ссылку на один раз и сохраните его вместо получения его каждый раз.</span><span class="sxs-lookup"><span data-stu-id="72cca-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="72cca-394">Получение контекста один раз гарантирует, что SignalR отправляет сообщения на клиентах в той же последовательности, в которой ваши методы концентратора выполнять клиентские вызовы методов.</span><span class="sxs-lookup"><span data-stu-id="72cca-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="72cca-395">Учебник, в котором показано, как использовать контекст SignalR для концентратора, см. в разделе [передача сообщений с сервера с помощью SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="72cca-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="72cca-396">Вызов методов клиента</span><span class="sxs-lookup"><span data-stu-id="72cca-396">Calling client methods</span></span>

<span data-ttu-id="72cca-397">Можно указать, какие клиенты будут получать RPC, но имеется меньше возможностей, чем при вызове из класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="72cca-398">Причиной этого является то, что контекст связан не конкретный вызов от клиента, поэтому любые методы, требуется знание текущий идентификатор соединения, такие как `Clients.Others`, или `Clients.Caller`, или `Clients.OthersInGroup`, недоступны.</span><span class="sxs-lookup"><span data-stu-id="72cca-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="72cca-399">Доступны следующие параметры.</span><span class="sxs-lookup"><span data-stu-id="72cca-399">The following options are available:</span></span>

- <span data-ttu-id="72cca-400">Все подключенные клиенты.</span><span class="sxs-lookup"><span data-stu-id="72cca-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="72cca-401">Конкретного клиента, определяемого по идентификатору подключения.</span><span class="sxs-lookup"><span data-stu-id="72cca-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="72cca-402">Все подключенные клиенты, кроме указанным клиентам, идентифицируемый идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="72cca-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="72cca-403">Все подключенные клиенты в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="72cca-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="72cca-404">Все подключенные клиенты в указанной группе, кроме указанным клиентам, идентифицируемый идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="72cca-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="72cca-405">При вызове в не Hub класс из методов в классе концентратора, вы можете передать в идентификатор текущего соединения и использовать его с `Clients.Client`, `Clients.AllExcept`, или `Clients.Group` для имитации `Clients.Caller`, `Clients.Others`, или `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="72cca-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="72cca-406">В следующем примере `MoveShapeHub` класс передает идентификатор подключения для `Broadcaster` класс таким образом, чтобы `Broadcaster` класса можно имитировать `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="72cca-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="72cca-407">Управление членством в группе</span><span class="sxs-lookup"><span data-stu-id="72cca-407">Managing group membership</span></span>

<span data-ttu-id="72cca-408">Для управления группами имеют одинаковые параметры, как в класс концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="72cca-409">Добавление клиента в группу</span><span class="sxs-lookup"><span data-stu-id="72cca-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="72cca-410">Удаление клиента из группы</span><span class="sxs-lookup"><span data-stu-id="72cca-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="72cca-411">Настройка конвейера концентраторов</span><span class="sxs-lookup"><span data-stu-id="72cca-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="72cca-412">SignalR позволяет ввести собственный код в конвейер концентратора.</span><span class="sxs-lookup"><span data-stu-id="72cca-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="72cca-413">В следующем примере показано пользовательский модуль конвейер концентратора, который регистрирует каждого входящего вызова метода, полученных от клиента и исходящий вызов метода, вызывается на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="72cca-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="72cca-414">В следующем коде в *Global.asax* файл регистрирует модуль для выполнения в конвейер концентратора:</span><span class="sxs-lookup"><span data-stu-id="72cca-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="72cca-415">Существует множество различных методов, которые можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="72cca-415">There are many different methods that you can override.</span></span> <span data-ttu-id="72cca-416">Полный список см. в разделе [HubPipelineModule методы](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="72cca-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
