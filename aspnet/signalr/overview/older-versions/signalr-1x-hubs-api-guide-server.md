---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: "Справочник по API концентраторов ASP.NET SignalR - сервера (SignalR 1.x) | Документы Microsoft"
author: pfletcher
description: "Этот документ содержит введение в программирование API концентраторов SignalR ASP.NET со стороны сервера для SignalR версии 1.1 с demonstratin образцы кода..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 96155b1c648e5f6092b3ba67a560197f86a593b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="00b79-103">Справочник по API концентраторов ASP.NET SignalR - сервера (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="00b79-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="00b79-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="00b79-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="00b79-105">Этот документ содержит введение в программирование API концентраторов SignalR ASP.NET со стороны сервера для версии 1.1, SignalR с образцы кода, демонстрирующие общих параметров.</span><span class="sxs-lookup"><span data-stu-id="00b79-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="00b79-106">API концентраторов SignalR позволяет вносить удаленные вызовы процедур (RPC) с сервера на подключенных клиентов и от клиентов к серверу.</span><span class="sxs-lookup"><span data-stu-id="00b79-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="00b79-107">В серверном коде определять методы, которые могут быть вызваны клиентов и вызова методов, которые выполняются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="00b79-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="00b79-108">В клиентском коде определяют методы, которые могут быть вызваны с сервера и вызывать методы, которые выполняются на сервере.</span><span class="sxs-lookup"><span data-stu-id="00b79-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="00b79-109">SignalR обеспечивает всех коммуникаций клиент сервер для вас.</span><span class="sxs-lookup"><span data-stu-id="00b79-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="00b79-110">SignalR также предлагает API нижнего уровня, который называется постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="00b79-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="00b79-111">Общие сведения о SignalR, концентраторов и постоянные подключения или учебник, в котором показано, как построить полное приложение SignalR см. [SignalR — начало работы](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="00b79-112">Обзор</span><span class="sxs-lookup"><span data-stu-id="00b79-112">Overview</span></span>

<span data-ttu-id="00b79-113">Этот документ содержит следующие разделы.</span><span class="sxs-lookup"><span data-stu-id="00b79-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="00b79-114">Как зарегистрировать SignalR маршрута и настройте параметры SignalR</span><span class="sxs-lookup"><span data-stu-id="00b79-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="00b79-115">URL-адрес /signalr</span><span class="sxs-lookup"><span data-stu-id="00b79-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="00b79-116">Настройка параметров SignalR</span><span class="sxs-lookup"><span data-stu-id="00b79-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="00b79-117">Создание и использование классов Hub</span><span class="sxs-lookup"><span data-stu-id="00b79-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="00b79-118">Время существования объекта концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="00b79-119">Верблюжий концентратора имен клиентов JavaScript</span><span class="sxs-lookup"><span data-stu-id="00b79-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="00b79-120">Несколько концентраторов</span><span class="sxs-lookup"><span data-stu-id="00b79-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="00b79-121">Как определить методы в классе концентратора, который клиенты смогут вызывать</span><span class="sxs-lookup"><span data-stu-id="00b79-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="00b79-122">Верблюжий имена методов в клиентов JavaScript</span><span class="sxs-lookup"><span data-stu-id="00b79-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="00b79-123">Если для асинхронного выполнения</span><span class="sxs-lookup"><span data-stu-id="00b79-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="00b79-124">Определение перегрузки</span><span class="sxs-lookup"><span data-stu-id="00b79-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="00b79-125">Способ вызова методов клиента от концентратора класса</span><span class="sxs-lookup"><span data-stu-id="00b79-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="00b79-126">При выборе какие клиенты будут получать RPC</span><span class="sxs-lookup"><span data-stu-id="00b79-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="00b79-127">Имена методов не проверяются во время компиляции</span><span class="sxs-lookup"><span data-stu-id="00b79-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="00b79-128">Совпадение имен метод без учета регистра</span><span class="sxs-lookup"><span data-stu-id="00b79-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="00b79-129">Асинхронное выполнение</span><span class="sxs-lookup"><span data-stu-id="00b79-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="00b79-130">Как управлять членством в группах от класса концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="00b79-131">Асинхронное выполнение методы Add и Remove</span><span class="sxs-lookup"><span data-stu-id="00b79-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="00b79-132">Сохраняемость членство группы</span><span class="sxs-lookup"><span data-stu-id="00b79-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="00b79-133">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="00b79-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="00b79-134">Как обрабатывать события времени жизни соединений в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="00b79-135">При вызове OnConnected, OnDisconnected и OnReconnected</span><span class="sxs-lookup"><span data-stu-id="00b79-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="00b79-136">Состояние вызывающего объекта не заполнен</span><span class="sxs-lookup"><span data-stu-id="00b79-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="00b79-137">Как получить информацию о клиенте из контекстного свойства</span><span class="sxs-lookup"><span data-stu-id="00b79-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="00b79-138">Передача состояния между клиентами и класс концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="00b79-139">Способ обработки ошибок в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="00b79-140">Как вызывать методы клиента групп и управление ими из вне класса концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="00b79-141">Вызов методов клиента</span><span class="sxs-lookup"><span data-stu-id="00b79-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="00b79-142">Управление членством в группе</span><span class="sxs-lookup"><span data-stu-id="00b79-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="00b79-143">Как включить трассировку</span><span class="sxs-lookup"><span data-stu-id="00b79-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="00b79-144">Настройка конвейера концентраторы</span><span class="sxs-lookup"><span data-stu-id="00b79-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="00b79-145">Для получения сведений о программе клиентов см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="00b79-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="00b79-146">Справочник по API концентраторов SignalR - клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="00b79-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="00b79-147">Справочник по API концентраторов SignalR - клиент .NET</span><span class="sxs-lookup"><span data-stu-id="00b79-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="00b79-148">Ссылки на разделы справки по API, до версии .NET 4.5 API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="00b79-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="00b79-149">Если вы используете .NET 4, см. раздел [версии .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="00b79-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="00b79-150">Как зарегистрировать SignalR маршрута и настройте параметры SignalR</span><span class="sxs-lookup"><span data-stu-id="00b79-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="00b79-151">Чтобы определить маршрут, который будет использоваться клиентами для подключения к концентратору, вызовите [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) метод при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="00b79-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="00b79-152">`MapHubs`— [метод расширения](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) для `System.Web.Routing.RouteCollection` класса.</span><span class="sxs-lookup"><span data-stu-id="00b79-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="00b79-153">В следующем примере показан способ определения маршрута концентраторы SignalR в *Global.asax* файла.</span><span class="sxs-lookup"><span data-stu-id="00b79-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="00b79-154">При добавлении SignalR функциональности в приложение ASP.NET MVC убедитесь, что маршрута SignalR добавляется раньше, чем другие маршруты.</span><span class="sxs-lookup"><span data-stu-id="00b79-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="00b79-155">Дополнительные сведения см. в разделе [учебник: Приступая к работе с SignalR и MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="00b79-156">URL-адрес /signalr</span><span class="sxs-lookup"><span data-stu-id="00b79-156">The /signalr URL</span></span>

<span data-ttu-id="00b79-157">По умолчанию используется URL-адрес маршрута, который клиенты будут использовать для подключения к концентратору «/ signalr».</span><span class="sxs-lookup"><span data-stu-id="00b79-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="00b79-158">(Не путайте этот URL-адрес с URL-адрес «/ signalr/концентраторов», предназначенная для автоматически созданный файл JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="00b79-159">Дополнительные сведения о созданном прокси см. в разделе [руководство по API концентраторов SignalR - клиент JavaScript - созданный прокси и делает за вас](index.md).)</span><span class="sxs-lookup"><span data-stu-id="00b79-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="00b79-160">Может быть непредвиденных обстоятельств, которые делают этот базовый URL-адрес не может использоваться для SignalR; Например, имеется папка с именем проекта *signalr* и вы не хотите изменить имя.</span><span class="sxs-lookup"><span data-stu-id="00b79-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="00b79-161">В этом случае можно изменить базовый URL-адрес, как показано в следующих примерах (Замените «/ signalr» в образце кода с нужный URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="00b79-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="00b79-162">**Код сервера, на который указывает URL-адрес**</span><span class="sxs-lookup"><span data-stu-id="00b79-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="00b79-163">**Клиентский код JavaScript, который указывает URL-адрес (с созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="00b79-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="00b79-164">**Клиентский код JavaScript, который указывает URL-адрес (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="00b79-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="00b79-165">**Клиентский код .NET, который указывает URL-адрес**</span><span class="sxs-lookup"><span data-stu-id="00b79-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="00b79-166">Настройка параметров SignalR</span><span class="sxs-lookup"><span data-stu-id="00b79-166">Configuring SignalR Options</span></span>

<span data-ttu-id="00b79-167">Перегруженные версии `MapHubs` метода позволяют указать настраиваемый URL-адрес, Сопоставитель пользовательскую зависимость и следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="00b79-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="00b79-168">Разрешить междоменные вызовы от клиентов браузера.</span><span class="sxs-lookup"><span data-stu-id="00b79-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="00b79-169">Как правило, если браузер загружает страницу из `http://contoso.com`, подключения SignalR находится в том же домене, в `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="00b79-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="00b79-170">Если страницы из `http://contoso.com` подключается к `http://fabrikam.com/signalr`, то есть соединение между доменами.</span><span class="sxs-lookup"><span data-stu-id="00b79-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="00b79-171">По соображениям безопасности соединения между доменами отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="00b79-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="00b79-172">Дополнительные сведения см. в разделе [ASP.NET руководство по API концентраторов SignalR - клиент JavaScript - как для установления соединения между доменами](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="00b79-173">Включите подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="00b79-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="00b79-174">При возникновении ошибки, по умолчанию для SignalR выполняется для отправки клиентам сообщение уведомления без подробной информации о том, что произошло.</span><span class="sxs-lookup"><span data-stu-id="00b79-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="00b79-175">Подробные сведения об ошибке отправки клиентам в рабочей среде, не рекомендуется, поскольку пользователей-злоумышленников, можно использовать информацию в атаки, направленные на ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="00b79-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="00b79-176">Сведения об устранении неполадок, можно использовать этот параметр, чтобы временно включить более информативные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="00b79-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="00b79-177">Отключите автоматически созданные файлы прокси JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="00b79-178">По умолчанию файл JavaScript с прокси-серверы для классов Hub создается в ответ на URL-адрес «/ signalr/концентраторов».</span><span class="sxs-lookup"><span data-stu-id="00b79-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="00b79-179">Если вы не хотите использовать прокси-серверы JavaScript, или если вы хотите создать этот файл вручную и ссылаться на физическом файле в клиентов, можно использовать этот параметр, чтобы отключить создание прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="00b79-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="00b79-180">Дополнительные сведения см. в разделе [руководство по API концентраторов SignalR - клиент JavaScript - прокси-сервера создается как создать физический файл для SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="00b79-181">Приведенный ниже показано, как указать URL-адрес подключения SignalR и эти параметры при вызове `MapHubs` метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="00b79-182">Чтобы задать пользовательский URL-адрес, замените «/ signalr» в примере на URL-адрес, который вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="00b79-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="00b79-183">Создание и использование классов Hub</span><span class="sxs-lookup"><span data-stu-id="00b79-183">How to create and use Hub classes</span></span>

<span data-ttu-id="00b79-184">Для создания концентратора, создайте класс, производный от [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="00b79-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="00b79-185">Следующий пример показывает простой класс концентратора для приложения разговора.</span><span class="sxs-lookup"><span data-stu-id="00b79-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="00b79-186">В этом примере можно вызвать подключенный клиент `NewContosoChatMessage` метода, и когда это происходит, данные, полученные широковещательной рассылки для всех подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="00b79-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="00b79-187">Время существования объекта концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-187">Hub object lifetime</span></span>

<span data-ttu-id="00b79-188">Не создавать экземпляр класса концентратора и вызывать его методы из собственного кода на сервере. все, что произведена за вас конвейером концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="00b79-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="00b79-189">SignalR создает новый экземпляр класса концентратора каждый раз, он должен обрабатывать операции концентратора, например, когда клиент подключается, отключает или выполняет вызов метода к серверу.</span><span class="sxs-lookup"><span data-stu-id="00b79-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="00b79-190">Так как экземпляры класса концентратора являются временными, их нельзя использовать для поддержания состояния от одного вызова метода к другому.</span><span class="sxs-lookup"><span data-stu-id="00b79-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="00b79-191">Каждый раз, сервер получает вызов метода из клиента, новый экземпляр класса процессов концентратора сообщения.</span><span class="sxs-lookup"><span data-stu-id="00b79-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="00b79-192">Для поддержания состояния через несколько соединений и вызовы методов, используйте другой метод, например базы данных или статическая переменная концентратора класса или другого класса, который является производным от `Hub`.</span><span class="sxs-lookup"><span data-stu-id="00b79-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="00b79-193">Если сохранить данные в памяти, с помощью метода, например статической переменной класса концентратора, данные будут потеряны при очистке домена приложения.</span><span class="sxs-lookup"><span data-stu-id="00b79-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="00b79-194">Если вы хотите отправлять сообщения для клиентов из собственного кода, который выполняется вне класса концентратора, просто нельзя сделать с помощью создания экземпляра класса концентратора, но это можно сделать путем получения ссылку на объект контекста SignalR для класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="00b79-195">Дополнительные сведения см. в разделе [как вызывать методы клиента групп и управление ими из вне класса концентратора](#callfromoutsidehub) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="00b79-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="00b79-196">Верблюжий концентратора имен клиентов JavaScript</span><span class="sxs-lookup"><span data-stu-id="00b79-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="00b79-197">По умолчанию клиенты JavaScript ссылаются концентраторов с использованием версии стиле Camel имени класса.</span><span class="sxs-lookup"><span data-stu-id="00b79-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="00b79-198">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="00b79-199">Предыдущий пример будет называться `contosoChatHub` в коде JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="00b79-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="00b79-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="00b79-201">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="00b79-202">Если вы хотите указать другое имя, чтобы клиенты могли использовать, добавьте `HubName` атрибута.</span><span class="sxs-lookup"><span data-stu-id="00b79-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="00b79-203">При использовании `HubName` атрибут, не изменяется имя верблюжий на клиентах JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="00b79-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="00b79-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="00b79-205">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="00b79-206">Несколько концентраторов</span><span class="sxs-lookup"><span data-stu-id="00b79-206">Multiple Hubs</span></span>

<span data-ttu-id="00b79-207">Можно определить несколько классов концентратора в приложении.</span><span class="sxs-lookup"><span data-stu-id="00b79-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="00b79-208">При этом общий доступ к подключению, но отдельные группы:</span><span class="sxs-lookup"><span data-stu-id="00b79-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="00b79-209">Все клиенты будут использовать же URL-адрес для подключения SignalR с вашей службой («/ signalr» или пользовательских URL-адрес, если вы указали одну), и что соединение используется для всех концентраторов определенной службой.</span><span class="sxs-lookup"><span data-stu-id="00b79-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="00b79-210">Нет никаких различий производительности для нескольких концентраторов по сравнению с определением в одном классе все функциональные возможности концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="00b79-211">Все концентраторы получить те же данные HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="00b79-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="00b79-212">Поскольку все концентраторы имеют то же подключение, только сведения о запросе HTTP, сервер возвращает — что поставляется в исходном запросе HTTP, устанавливает соединение SignalR.</span><span class="sxs-lookup"><span data-stu-id="00b79-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="00b79-213">Если вы используете запрос на подключение для передачи данных от клиента к серверу, указав строку запроса, не может предоставить различные строки запросов в разных концентраторы.</span><span class="sxs-lookup"><span data-stu-id="00b79-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="00b79-214">Все концентраторы будут получать те же сведения.</span><span class="sxs-lookup"><span data-stu-id="00b79-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="00b79-215">Созданный файл прокси JavaScript будет содержать учетные записи-посредники для всех концентраторов в одном файле.</span><span class="sxs-lookup"><span data-stu-id="00b79-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="00b79-216">Сведения о прокси-серверы JavaScript см. в разделе [руководство по API концентраторов SignalR - клиент JavaScript - созданный прокси и делает за вас](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="00b79-217">Группы определяются в пределах концентраторов.</span><span class="sxs-lookup"><span data-stu-id="00b79-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="00b79-218">В SignalR, которые можно определить имя группы для передачи на подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="00b79-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="00b79-219">Группы поддерживаются отдельно для каждого концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="00b79-220">Например, группа «Администраторы» включить один набор клиентов для вашего `ContosoChatHub` относится к другому набору клиентов для класса и тем же именем вашего `StockTickerHub` класса.</span><span class="sxs-lookup"><span data-stu-id="00b79-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="00b79-221">Как определить методы в классе концентратора, который клиенты смогут вызывать</span><span class="sxs-lookup"><span data-stu-id="00b79-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="00b79-222">Чтобы предоставить метод на концентраторе, должен быть вызван из клиента, объявите открытый метод, как показано в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="00b79-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="00b79-223">Можно указать тип возвращаемого значения и параметры, включая сложные типы массивов и, как и в любой метод C#.</span><span class="sxs-lookup"><span data-stu-id="00b79-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="00b79-224">Все данные, в параметрах получения или возврата вызывающему передается между клиентом и сервером с помощью JSON и SignalR обрабатывает привязки сложные объекты и массивы объектов автоматически.</span><span class="sxs-lookup"><span data-stu-id="00b79-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="00b79-225">Верблюжий имена методов в клиентов JavaScript</span><span class="sxs-lookup"><span data-stu-id="00b79-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="00b79-226">По умолчанию клиенты JavaScript ссылаются методов концентратора с использованием версии стиле Camel имени метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="00b79-227">SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="00b79-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="00b79-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="00b79-229">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="00b79-230">Если вы хотите указать другое имя, чтобы клиенты могли использовать, добавьте `HubMethodName` атрибута.</span><span class="sxs-lookup"><span data-stu-id="00b79-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="00b79-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="00b79-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="00b79-232">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="00b79-233">Если для асинхронного выполнения</span><span class="sxs-lookup"><span data-stu-id="00b79-233">When to execute asynchronously</span></span>

<span data-ttu-id="00b79-234">Если метод будет иметь длительные или должен работать, следует включать ожидания, например, поиск в базе данных или вызов веб-службы, создания асинхронного метода концентратора, возвращая [задачи](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (вместо `void` возврата) или [ Задача&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) объектов (вместо `T` тип возвращаемого значения).</span><span class="sxs-lookup"><span data-stu-id="00b79-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="00b79-235">При возвращении `Task` ожидает объект из метода SignalR `Task` для завершения, а затем отправляет распаковать результат обратно клиенту, поэтому нет никаких различий в том, как код вызова метода в клиенте.</span><span class="sxs-lookup"><span data-stu-id="00b79-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="00b79-236">Создание метода концентратора асинхронной позволяет избежать блокировать подключение, если она использует транспорт WebSocket.</span><span class="sxs-lookup"><span data-stu-id="00b79-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="00b79-237">Если транспортом является WebSocket метод концентратора выполняется синхронно, последующие вызовы методов в концентраторе от одного клиента, блокируются до завершения выполнения метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="00b79-238">В следующем примере показано, тот же метод в коде выполняются синхронно или асинхронно, а затем клиентский код JavaScript, который работает для вызова любой версии.</span><span class="sxs-lookup"><span data-stu-id="00b79-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="00b79-239">**Synchronous**</span><span class="sxs-lookup"><span data-stu-id="00b79-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="00b79-240">**Асинхронные - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="00b79-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="00b79-241">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="00b79-242">Дополнительные сведения об использовании асинхронных методов в ASP.NET 4.5 см. в разделе [использование асинхронных методов в ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="00b79-243">Определение перегрузки</span><span class="sxs-lookup"><span data-stu-id="00b79-243">Defining Overloads</span></span>

<span data-ttu-id="00b79-244">Если вы хотите определить перегрузки для метода, число параметров в каждой перегрузке должны быть разными.</span><span class="sxs-lookup"><span data-stu-id="00b79-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="00b79-245">Если перегрузка отличить просто задав разные типы параметров, класс концентратора будет скомпилирован, но службы SignalR будет вызывать исключение во время выполнения, когда клиенты пытаются вызов перегрузки.</span><span class="sxs-lookup"><span data-stu-id="00b79-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="00b79-246">Способ вызова методов клиента от концентратора класса</span><span class="sxs-lookup"><span data-stu-id="00b79-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="00b79-247">Для вызова методов клиента с сервера, используйте `Clients` свойство метод в классе концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="00b79-248">В следующем примере показано серверного кода, который вызывает `addNewMessageToPage` на всех подключенных клиентов и клиентский код, который определяет метод, в клиент JavaScript.</span><span class="sxs-lookup"><span data-stu-id="00b79-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="00b79-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="00b79-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="00b79-250">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="00b79-251">Не удается получить значение, возвращаемое из метода клиента; синтаксис, такие как `int x = Clients.All.add(1,1)` не работает.</span><span class="sxs-lookup"><span data-stu-id="00b79-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="00b79-252">Можно указать, сложные типы и массивы параметров.</span><span class="sxs-lookup"><span data-stu-id="00b79-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="00b79-253">В следующем примере передается сложный тип клиенту в параметре метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="00b79-254">**Код сервера, который вызывает клиентский метод, с помощью сложного объекта**</span><span class="sxs-lookup"><span data-stu-id="00b79-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="00b79-255">**Код сервера, который определяет сложный объект**</span><span class="sxs-lookup"><span data-stu-id="00b79-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="00b79-256">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="00b79-257">При выборе какие клиенты будут получать RPC</span><span class="sxs-lookup"><span data-stu-id="00b79-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="00b79-258">Это свойство возвращает клиентов [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) объект, который предоставляет несколько параметров для указания, какие клиенты получат RPC:</span><span class="sxs-lookup"><span data-stu-id="00b79-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="00b79-259">Все подключенные клиенты.</span><span class="sxs-lookup"><span data-stu-id="00b79-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="00b79-260">Вызывающий клиент.</span><span class="sxs-lookup"><span data-stu-id="00b79-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="00b79-261">Все клиенты, кроме вызывающего клиента.</span><span class="sxs-lookup"><span data-stu-id="00b79-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="00b79-262">Конкретным клиентом, определяется идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="00b79-263">В этом примере вызывается `addContosoChatMessageToPage` на вызывающего клиента и действует так же, как с помощью `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="00b79-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="00b79-264">Все подключенные клиенты, кроме указанным клиентам, определяемый по идентификатору соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="00b79-265">Все подключенные клиенты в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="00b79-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="00b79-266">Все подключенные клиенты в указанной группе, кроме указанным клиентам, определяемый по идентификатору соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="00b79-267">Все подключенные клиенты в указанной группе, кроме вызывающего клиента.</span><span class="sxs-lookup"><span data-stu-id="00b79-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="00b79-268">Имена методов не проверяются во время компиляции</span><span class="sxs-lookup"><span data-stu-id="00b79-268">No compile-time validation for method names</span></span>

<span data-ttu-id="00b79-269">Имя метода, указываемое интерпретируется как динамический объект, это означает, что нет IntelliSense или проверки во время компиляции для него.</span><span class="sxs-lookup"><span data-stu-id="00b79-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="00b79-270">Выражение вычисляется во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="00b79-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="00b79-271">При вызове метода SignalR отправляет имя метода и значения параметров клиента, а если клиент содержит метод, который совпадает с именем, что вызывается метод и значения параметров передаются в него.</span><span class="sxs-lookup"><span data-stu-id="00b79-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="00b79-272">При обнаружении на стороне клиента нет соответствующего метода ошибка не возникает.</span><span class="sxs-lookup"><span data-stu-id="00b79-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="00b79-273">Сведения о формате данных, SignalR передает клиенту за кулисами, при вызове метода клиента см. в разделе [Общие сведения о SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="00b79-274">Совпадение имен метод без учета регистра</span><span class="sxs-lookup"><span data-stu-id="00b79-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="00b79-275">Метод совпадение имен регистр не учитывается.</span><span class="sxs-lookup"><span data-stu-id="00b79-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="00b79-276">Например `Clients.All.addContosoChatMessageToPage` на сервере будет выполняться `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, или `addContosoChatMessageToPage` на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="00b79-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="00b79-277">Асинхронное выполнение</span><span class="sxs-lookup"><span data-stu-id="00b79-277">Asynchronous execution</span></span>

<span data-ttu-id="00b79-278">Метод, вызываемый выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="00b79-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="00b79-279">Любой код, который поставляется после вызова метода для клиента будет выполняться немедленно без ожидания SignalR для завершения передачи данных на клиентов, если не указать, что следующие строки кода следует подождать завершения метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="00b79-280">В следующем примере кода показано, как для последовательного выполнения двух методов клиента, его с помощью кода, работы с .NET 4.5 и один с помощью кода, работы с .NET 4.</span><span class="sxs-lookup"><span data-stu-id="00b79-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="00b79-281">**Пример .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="00b79-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="00b79-282">**Пример .NET 4**</span><span class="sxs-lookup"><span data-stu-id="00b79-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="00b79-283">Если вы используете `await` или `ContinueWith` подождать, пока клиентский метод завершения до выполнения следующей строке кода, это не значит, клиенты фактически будут получать сообщение перед выполнением следующей строке кода.</span><span class="sxs-lookup"><span data-stu-id="00b79-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="00b79-284">Вызов клиентского метода «завершения» означает только то SignalR выполнил все компоненты, необходимые для отправки сообщения.</span><span class="sxs-lookup"><span data-stu-id="00b79-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="00b79-285">Если требуется проверка, что клиенты получили сообщение, необходимо самостоятельно программы такой механизм.</span><span class="sxs-lookup"><span data-stu-id="00b79-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="00b79-286">Например, можно код `MessageReceived` метод на концентраторе и в `addContosoChatMessageToPage` метод на стороне клиента, можно вызвать `MessageReceived` после этого любые работала должен выполнить на клиенте.</span><span class="sxs-lookup"><span data-stu-id="00b79-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="00b79-287">В `MessageReceived` в концентраторе можно сделать, независимо от работы зависит от клиента фактическое получение и обработка вызова исходного метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="00b79-288">Использование строковой переменной в качестве имени метода</span><span class="sxs-lookup"><span data-stu-id="00b79-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="00b79-289">Если необходимо вызвать метод клиента с помощью строковой переменной как имя метода, приведение `Clients.All` (или `Clients.Others`, `Clients.Caller`, т. д.) для `IClientProxy` и затем вызвать [Invoke (имя_метода, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="00b79-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="00b79-290">Как управлять членством в группах от класса концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="00b79-291">Группы в SignalR предоставляют метод для широковещательной рассылки сообщений для указанного подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="00b79-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="00b79-292">Группа может иметь любое количество клиентов, и клиент может быть членом любое количество групп.</span><span class="sxs-lookup"><span data-stu-id="00b79-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="00b79-293">Чтобы управлять членством в группах, используйте [добавить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) и [удалить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) методы, предоставляемые `Groups` свойство класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="00b79-294">В следующем примере показан `Groups.Add` и `Groups.Remove` методы, которые используются в методах Hub, которые вызываются из клиентского кода, а затем клиентский код JavaScript, который вызывает их.</span><span class="sxs-lookup"><span data-stu-id="00b79-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="00b79-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="00b79-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="00b79-296">**С помощью созданного прокси клиента JavaScript**</span><span class="sxs-lookup"><span data-stu-id="00b79-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="00b79-297">Не нужно явно создавать группы.</span><span class="sxs-lookup"><span data-stu-id="00b79-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="00b79-298">Фактически группа автоматически создается впервые, укажите его имя в вызове `Groups.Add`, и удаляется при удалении последнего соединения через членство в ней.</span><span class="sxs-lookup"><span data-stu-id="00b79-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="00b79-299">Нет никакой интерфейс API для получения список членства в группе или список групп.</span><span class="sxs-lookup"><span data-stu-id="00b79-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="00b79-300">SignalR отправляет сообщения клиентам и группам на основе [модели pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), и не поддерживает списки групп или членства в группах.</span><span class="sxs-lookup"><span data-stu-id="00b79-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="00b79-301">Это позволяет увеличить масштабируемость, так как каждый раз при добавлении узла к веб-ферме, любое состояние, которое поддерживает SignalR для нового узла.</span><span class="sxs-lookup"><span data-stu-id="00b79-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="00b79-302">Асинхронное выполнение методы Add и Remove</span><span class="sxs-lookup"><span data-stu-id="00b79-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="00b79-303">`Groups.Add` И `Groups.Remove` методы асинхронного выполнения.</span><span class="sxs-lookup"><span data-stu-id="00b79-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="00b79-304">Если вы хотите добавить в группу клиента и немедленно отправить клиенту сообщение с помощью группы, необходимо убедиться, что `Groups.Add` метод завершается первой.</span><span class="sxs-lookup"><span data-stu-id="00b79-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="00b79-305">В следующем примере кода показано, как сделать это с помощью кода, который работает в .NET 4.5, а другой с помощью кода, который работает в .NET 4</span><span class="sxs-lookup"><span data-stu-id="00b79-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="00b79-306">**Пример .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="00b79-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="00b79-307">**Пример .NET 4**</span><span class="sxs-lookup"><span data-stu-id="00b79-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="00b79-308">Сохраняемость членство группы</span><span class="sxs-lookup"><span data-stu-id="00b79-308">Group membership persistence</span></span>

<span data-ttu-id="00b79-309">SignalR отслеживает соединения, не пользователей, поэтому, если требуется, чтобы пользователю не проходить в той же группе каждый раз, когда пользователь выполняет подключение, необходимо вызвать `Groups.Add` каждый раз, когда пользователь выполняет новое подключение.</span><span class="sxs-lookup"><span data-stu-id="00b79-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="00b79-310">После временной потери связи иногда SignalR можно восстановить подключение автоматически.</span><span class="sxs-lookup"><span data-stu-id="00b79-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="00b79-311">В этом случае SignalR восстановление то же соединение без создания нового подключения и таким образом, членство в группе клиентских восстанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="00b79-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="00b79-312">Это возможно даже в том случае, если временной приостановки выполнения не в результате перезагрузки сервера или сбой, так как состояние соединения для каждого клиента, в том числе членства в группах, обхода клиенту.</span><span class="sxs-lookup"><span data-stu-id="00b79-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="00b79-313">Если сервер выйдет из строя и заменяется на новый сервер до истечения времени ожидания соединения, клиент может автоматически подключаться к новому серверу и повторите регистрацию в группах, в которую он входит.</span><span class="sxs-lookup"><span data-stu-id="00b79-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="00b79-314">Когда соединение невозможно восстановить автоматически после потери подключения, или если время ожидания соединения или при отключении клиента (например, когда браузер переходит на новую страницу), членство в группах будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="00b79-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="00b79-315">При очередном подключении будет новое соединение.</span><span class="sxs-lookup"><span data-stu-id="00b79-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="00b79-316">Для поддержки членства в группах, когда пользователь устанавливает новое соединение, приложение должно отслеживать связи между пользователями и группами, а также восстановить членства в группах каждый раз пользователь устанавливает новое соединение.</span><span class="sxs-lookup"><span data-stu-id="00b79-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="00b79-317">Дополнительные сведения о подключениях и переподключения см. в разделе [способ обработки событий время существования подключения в классе концентратора](#connectionlifetime) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="00b79-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="00b79-318">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="00b79-318">Single-user groups</span></span>

<span data-ttu-id="00b79-319">Приложения, использующие SignalR обычно имеют для отслеживания сопоставлений пользователей и подключений, чтобы узнать, какой пользователь отправил сообщение и какие пользователи должны быть способны получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="00b79-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="00b79-320">Группы используются в одном из двух шаблонов, часто используемых для соответствующей.</span><span class="sxs-lookup"><span data-stu-id="00b79-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="00b79-321">Группы в однопользовательском режиме.</span><span class="sxs-lookup"><span data-stu-id="00b79-321">Single-user groups.</span></span>

    <span data-ttu-id="00b79-322">Укажите имя пользователя в качестве имени группы и добавление в группу идентификатор текущего подключения каждый раз пользователь подключается или повторном подключении.</span><span class="sxs-lookup"><span data-stu-id="00b79-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="00b79-323">Для отправки сообщений пользователю отправлять в группу.</span><span class="sxs-lookup"><span data-stu-id="00b79-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="00b79-324">Недостаток этого метода заключается в том, группе не предоставляют способ выяснить, является ли пользователь сети или вне сети.</span><span class="sxs-lookup"><span data-stu-id="00b79-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="00b79-325">Отслеживать взаимосвязи между имена пользователей и идентификаторов подключений.</span><span class="sxs-lookup"><span data-stu-id="00b79-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="00b79-326">Можно сохранить ассоциацию между именами пользователей и один или несколько идентификаторов подключений в словарь или базы данных и обновить сохраненные данные при каждом его подключении или отключении.</span><span class="sxs-lookup"><span data-stu-id="00b79-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="00b79-327">Для отправки сообщений пользователю указать идентификаторов подключений.</span><span class="sxs-lookup"><span data-stu-id="00b79-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="00b79-328">Недостатком этого метода является то, что требуется больше памяти.</span><span class="sxs-lookup"><span data-stu-id="00b79-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="00b79-329">Как обрабатывать события времени жизни соединений в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="00b79-330">Типичные причины для обработки события времени жизни соединения: для отслеживания ли пользователь должен быть подключен или не и для отслеживания сопоставление имен пользователей и идентификаторов подключений.</span><span class="sxs-lookup"><span data-stu-id="00b79-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="00b79-331">Для выполнения собственного кода клиентам подключать или отключать, переопределите `OnConnected`, `OnDisconnected`, и `OnReconnected` виртуальные методы концентратора класса, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="00b79-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="00b79-332">При вызове OnConnected, OnDisconnected и OnReconnected</span><span class="sxs-lookup"><span data-stu-id="00b79-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="00b79-333">Каждый раз, браузер переходит на новую страницу, новое соединение имеет должно устанавливаться, это означает, что будет выполняться SignalR `OnDisconnected` метода, за которым следует `OnConnected` метод.</span><span class="sxs-lookup"><span data-stu-id="00b79-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="00b79-334">SignalR всегда создает новый идентификатор соединения, если установлено новое соединение.</span><span class="sxs-lookup"><span data-stu-id="00b79-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="00b79-335">`OnReconnected` Метод вызывается, когда была временной разрыв подключения, можно автоматически восстановить SignalR, например при кабелем временно отключен и использовать подключение до истечения времени ожидания соединения. `OnDisconnected` Метод вызывается, когда клиент отключен и SignalR не может автоматически подключаться, например когда браузер переходит на новую страницу.</span><span class="sxs-lookup"><span data-stu-id="00b79-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="00b79-336">Таким образом, возможно последовательность событий для данного клиента — `OnConnected`, `OnReconnected`, `OnDisconnected`; или `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="00b79-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="00b79-337">Вы не увидите последовательность `OnConnected`, `OnDisconnected`, `OnReconnected` для данного соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="00b79-338">`OnDisconnected` Домен приложения получает перезапущен или метод не вызывается в некоторых сценариях, например при отказе одного сервера.</span><span class="sxs-lookup"><span data-stu-id="00b79-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="00b79-339">При другой сервер переходит в оперативный или домен приложений завершает его повторный запуск, некоторые клиенты могут быть возможность повторного подключения и инициировать `OnReconnected` событий.</span><span class="sxs-lookup"><span data-stu-id="00b79-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="00b79-340">Дополнительные сведения см. в разделе [понимание и обработка события времени жизни соединений в SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="00b79-341">Состояние вызывающего объекта не заполнен</span><span class="sxs-lookup"><span data-stu-id="00b79-341">Caller state not populated</span></span>

<span data-ttu-id="00b79-342">Методы обработчиков событий времени существования соединения вызываются на сервере, это означает, что любое состояние, которое необходимо поместить в `state` объекта на стороне клиента не заполняется в `Caller` свойство на сервере.</span><span class="sxs-lookup"><span data-stu-id="00b79-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="00b79-343">Сведения о `state` объекта и `Caller` свойство, в разделе [передача состояния между клиентами и классу Hub](#passstate) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="00b79-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="00b79-344">Как получить информацию о клиенте из контекстного свойства</span><span class="sxs-lookup"><span data-stu-id="00b79-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="00b79-345">Чтобы получить сведения о клиенте, используйте `Context` свойство класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="00b79-346">`Context` Возвращает [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) объект, который предоставляет доступ к следующим данным:</span><span class="sxs-lookup"><span data-stu-id="00b79-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="00b79-347">Идентификатор подключения вызывающего клиента.</span><span class="sxs-lookup"><span data-stu-id="00b79-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="00b79-348">Идентификатор подключения — это GUID, назначаемый SignalR (в собственном коде невозможно задать значение).</span><span class="sxs-lookup"><span data-stu-id="00b79-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="00b79-349">Имеется один идентификатор подключения для каждого подключения и то же подключение, при наличии нескольких концентраторов в своем приложении код используется всех концентраторов.</span><span class="sxs-lookup"><span data-stu-id="00b79-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="00b79-350">Данные заголовка HTTP.</span><span class="sxs-lookup"><span data-stu-id="00b79-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="00b79-351">Можно также получить HTTP-заголовки от `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="00b79-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="00b79-352">Несколько ссылок на одно и то же самое обусловлено тем, `Context.Headers` была создана, во-первых, `Context.Request` свойство было добавлено более поздней версии, и `Context.Headers` было сохранено для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="00b79-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="00b79-353">Запрос данных строки.</span><span class="sxs-lookup"><span data-stu-id="00b79-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="00b79-354">Можно также получить строку запроса из `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="00b79-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="00b79-355">Строка запроса, вы получаете в это свойство является тот, который был использован с HTTP-запроса, установленного подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="00b79-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="00b79-356">Можно добавлять параметры строки запроса в клиенте, настраивая соединение, которое является удобным способом для передачи данных о клиенте от клиента к серверу.</span><span class="sxs-lookup"><span data-stu-id="00b79-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="00b79-357">В примере показан один из способов добавления строки запроса в клиенте JavaScript при использовании созданного прокси.</span><span class="sxs-lookup"><span data-stu-id="00b79-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="00b79-358">Дополнительные сведения о настройке параметров строки запроса см. в разделе руководства по API для [JavaScript](index.md) и [.NET](index.md) клиентов.</span><span class="sxs-lookup"><span data-stu-id="00b79-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="00b79-359">Можно найти метод транспорта, используемый для соединения в данные строки запроса, а также некоторые значения, используются внутренними механизмами SignalR:</span><span class="sxs-lookup"><span data-stu-id="00b79-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="00b79-360">Значение `transportMethod` будет «webSockets», «serverSentEvents», «foreverFrame» или «longPolling».</span><span class="sxs-lookup"><span data-stu-id="00b79-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="00b79-361">Обратите внимание, что если вы установите это значение `OnConnected` метод обработчика событий, в некоторых сценариях может изначально появиться транспорта значение, которое не является метод окончательного согласованный транспорта для подключения.</span><span class="sxs-lookup"><span data-stu-id="00b79-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="00b79-362">В этом случае метод вызовет исключение и будет вызван позже при установлении метод окончательного транспорта.</span><span class="sxs-lookup"><span data-stu-id="00b79-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="00b79-363">Файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="00b79-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="00b79-364">Можно также получить файлы cookie из `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="00b79-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="00b79-365">Сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="00b79-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="00b79-366">Объект HttpContext для запроса:</span><span class="sxs-lookup"><span data-stu-id="00b79-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="00b79-367">Используйте этот метод вместо `HttpContext.Current` для получения `HttpContext` объект для подключения SignalR.</span><span class="sxs-lookup"><span data-stu-id="00b79-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="00b79-368">Передача состояния между клиентами и класс концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="00b79-369">Предоставляет клиентский прокси-класс `state` объекта, в котором можно хранить данные, необходимые для передачи на сервер с каждым вызовом метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="00b79-370">На сервере можно получить доступ к данным в `Clients.Caller` свойство в методах Hub, которые называются клиентами.</span><span class="sxs-lookup"><span data-stu-id="00b79-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="00b79-371">`Clients.Caller` Свойство не заполняется для методы обработки событий время существования подключения `OnConnected`, `OnDisconnected`, и `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="00b79-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="00b79-372">Создание или обновление данных в `state` объекта и `Clients.Caller` свойство работает в обоих направлениях.</span><span class="sxs-lookup"><span data-stu-id="00b79-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="00b79-373">Можно обновить значения на сервере, и они передаются обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="00b79-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="00b79-374">В следующем примере клиентский код JavaScript, которая сохраняет состояние для передачи на сервер с каждым вызовом метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="00b79-375">В следующем примере показан эквивалентный код в клиент .NET.</span><span class="sxs-lookup"><span data-stu-id="00b79-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="00b79-376">В классе концентратора, можно получить доступ к эти данные в `Clients.Caller` свойство.</span><span class="sxs-lookup"><span data-stu-id="00b79-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="00b79-377">В примере показан код, который получает состояние говорится в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="00b79-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="00b79-378">Этот механизм для сохранения состояния не предназначен для больших объемов данных, поскольку все символы можно поместить в `state` или `Clients.Caller` свойства обхода с каждого вызова метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="00b79-379">Это полезно для небольших элементов, таких как имена пользователей и счетчики.</span><span class="sxs-lookup"><span data-stu-id="00b79-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="00b79-380">Способ обработки ошибок в классе концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="00b79-381">Для обработки ошибок, возникающих в методах класса концентратора, используйте один или оба из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="00b79-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="00b79-382">Перенос кода метода в блоки try-catch и войдите в объект исключения.</span><span class="sxs-lookup"><span data-stu-id="00b79-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="00b79-383">В целях отладки исключения можно отправить клиенту, но для обеспечения безопасности причин отправки подробные сведения на клиентах в рабочей среде не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="00b79-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="00b79-384">Создание модуля концентраторов конвейера, который будет обрабатывать [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) метод.</span><span class="sxs-lookup"><span data-stu-id="00b79-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="00b79-385">В следующем примере показано конвейера модуль, который регистрирует ошибки, Далее следует код в файле Global.asax, которое вставляет модуль в конвейере концентраторов.</span><span class="sxs-lookup"><span data-stu-id="00b79-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="00b79-386">Дополнительные сведения о модулях конвейер концентратора см. в разделе [Настройка конвейера концентраторов](#hubpipeline) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="00b79-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="00b79-387">Как включить трассировку</span><span class="sxs-lookup"><span data-stu-id="00b79-387">How to enable tracing</span></span>

<span data-ttu-id="00b79-388">Чтобы включить трассировку на стороне сервера, добавьте элемент system.diagnostics файла Web.config, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="00b79-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="00b79-389">При запуске приложения в Visual Studio, можно просмотреть журналы в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="00b79-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="00b79-390">Как вызывать методы клиента групп и управление ими из вне класса концентратора</span><span class="sxs-lookup"><span data-stu-id="00b79-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="00b79-391">Для вызова методов клиента от другого класса от класса концентратора, получить ссылку на объект контекста SignalR для концентратора и используйте его для вызова методов на стороне клиента и управление группами.</span><span class="sxs-lookup"><span data-stu-id="00b79-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="00b79-392">Следующий пример `StockTicker` класса возвращает объект контекста, сохраняет его в экземпляр класса, сохраняет экземпляр класса в статическое свойство и использует контекст из одноэлементный экземпляр класса для вызова `updateStockPrice` метод на клиентах, которые являются подключение к концентратору с именем `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="00b79-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="00b79-393">Если необходимо использовать время несколько контекста в долгоживущих объектов, получить ссылку на один раз и сохраните его, а не получение файла каждый раз.</span><span class="sxs-lookup"><span data-stu-id="00b79-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="00b79-394">После получения контекста гарантирует, что SignalR отправляет сообщения на клиентах в той же последовательности, в которой свои методы концентратора выполнять клиентские вызовы метода.</span><span class="sxs-lookup"><span data-stu-id="00b79-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="00b79-395">Учебник, в котором показано, как использовать контекст SignalR концентратора, в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="00b79-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="00b79-396">Вызов методов клиента</span><span class="sxs-lookup"><span data-stu-id="00b79-396">Calling client methods</span></span>

<span data-ttu-id="00b79-397">Можно указать, какие клиенты получат RPC, но имеется меньше возможностей, чем при вызове из класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="00b79-398">Это обусловлено тем, контекст не поддерживается связанной с определенной вызов от клиента, любые методы, требуется знание идентификатор текущего соединения, такие как `Clients.Others`, или `Clients.Caller`, или `Clients.OthersInGroup`, недоступны.</span><span class="sxs-lookup"><span data-stu-id="00b79-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="00b79-399">Доступны следующие параметры.</span><span class="sxs-lookup"><span data-stu-id="00b79-399">The following options are available:</span></span>

- <span data-ttu-id="00b79-400">Все подключенные клиенты.</span><span class="sxs-lookup"><span data-stu-id="00b79-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="00b79-401">Конкретным клиентом, определяется идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="00b79-402">Все подключенные клиенты, кроме указанным клиентам, определяемый по идентификатору соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="00b79-403">Все подключенные клиенты в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="00b79-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="00b79-404">Все подключенные клиенты в указанной группе, кроме указанным клиентам, определяемый по идентификатору соединения.</span><span class="sxs-lookup"><span data-stu-id="00b79-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="00b79-405">При вызове в класс без концентратора с помощью методов в классе концентратора, можно передать идентификатор текущего соединения и использовать его с `Clients.Client`, `Clients.AllExcept`, или `Clients.Group` для имитации `Clients.Caller`, `Clients.Others`, или `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="00b79-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="00b79-406">В следующем примере `MoveShapeHub` класс передает идентификатор соединения для `Broadcaster` класса, чтобы `Broadcaster` имитирует класс `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="00b79-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="00b79-407">Управление членством в группе</span><span class="sxs-lookup"><span data-stu-id="00b79-407">Managing group membership</span></span>

<span data-ttu-id="00b79-408">Для управления группами имеют те же параметры, как в класс концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="00b79-409">Добавление клиента в группу</span><span class="sxs-lookup"><span data-stu-id="00b79-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="00b79-410">Удаление клиента из группы</span><span class="sxs-lookup"><span data-stu-id="00b79-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="00b79-411">Настройка конвейера концентраторы</span><span class="sxs-lookup"><span data-stu-id="00b79-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="00b79-412">SignalR позволяет ввести собственный код в конвейер концентратора.</span><span class="sxs-lookup"><span data-stu-id="00b79-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="00b79-413">Ниже приведен пример пользовательский модуль конвейер концентратора, который регистрирует каждого входящего вызова метода, полученные от клиента и исходящий вызов метод вызван на клиенте.</span><span class="sxs-lookup"><span data-stu-id="00b79-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="00b79-414">В следующем примере кода в *Global.asax* файл регистрирует модуль для выполнения в конвейер концентратора:</span><span class="sxs-lookup"><span data-stu-id="00b79-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="00b79-415">Существует множество различных методов, которые можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="00b79-415">There are many different methods that you can override.</span></span> <span data-ttu-id="00b79-416">Полный список см. в разделе [HubPipelineModule методы](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="00b79-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
