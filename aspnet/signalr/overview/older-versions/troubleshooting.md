---
uid: signalr/overview/older-versions/troubleshooting
title: Устранение неполадок SignalR (SignalR 1.x) | Документы Microsoft
author: pfletcher
description: В этой статье описываются распространенных проблем в разработке приложений SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505823"
---
<a name="signalr-troubleshooting-signalr-1x"></a><span data-ttu-id="4eace-103">Устранение неполадок SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="4eace-103">SignalR Troubleshooting (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4eace-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4eace-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="4eace-105">В этом документе описываются распространенные проблемы, связанные с SignalR.</span><span class="sxs-lookup"><span data-stu-id="4eace-105">This document describes common troubleshooting issues with SignalR.</span></span>


<span data-ttu-id="4eace-106">В этом документе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="4eace-106">This document contains the following sections.</span></span>

- [<span data-ttu-id="4eace-107">Вызов методов между клиентом и сервером без вмешательства пользователя завершается с ошибкой</span><span class="sxs-lookup"><span data-stu-id="4eace-107">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="4eace-108">Другие проблемы с подключением</span><span class="sxs-lookup"><span data-stu-id="4eace-108">Other connection issues</span></span>](#other)
- [<span data-ttu-id="4eace-109">Ошибки компиляции и на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="4eace-109">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="4eace-110">Проблемы Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4eace-110">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="4eace-111">Проблемы в службах IIS</span><span class="sxs-lookup"><span data-stu-id="4eace-111">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="4eace-112">Проблемы Azure</span><span class="sxs-lookup"><span data-stu-id="4eace-112">Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="4eace-113">Вызов методов между клиентом и сервером без вмешательства пользователя завершается с ошибкой</span><span class="sxs-lookup"><span data-stu-id="4eace-113">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="4eace-114">В этом разделе описываются возможные причины между клиентом и сервером, понятное сообщение в случае сбоя вызова метода.</span><span class="sxs-lookup"><span data-stu-id="4eace-114">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="4eace-115">В приложении SignalR сервер не содержит информации о методах, которые реализует клиента; При сервер вызывает клиентский метод, метод имени и параметров данные отправляются клиенту, и этот метод выполняется только в том случае, если он существует в формат, указанный сервер.</span><span class="sxs-lookup"><span data-stu-id="4eace-115">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="4eace-116">Если нет соответствующего метода находится на стороне клиента, ничего не происходит, и сообщение об ошибке не возникает на сервере.</span><span class="sxs-lookup"><span data-stu-id="4eace-116">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="4eace-117">Для дальнейшей диагностики клиента методов не вызван, можно включить ведение журнала перед вызовом метода, метод start в концентраторе отображать какие вызовы приходящие с сервера.</span><span class="sxs-lookup"><span data-stu-id="4eace-117">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="4eace-118">Включение ведения журнала в приложении JavaScript, в разделе [как включить ведение журнала на стороне клиента (версия клиента JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="4eace-118">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="4eace-119">Чтобы включить ведение журнала в клиентском приложении .NET, в разделе [как включить ведение журнала на стороне клиента (версия клиента .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="4eace-119">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="4eace-120">Неправильно написанное метода, Неверный метод подписи или имя неверные концентратора</span><span class="sxs-lookup"><span data-stu-id="4eace-120">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="4eace-121">Если соответствующий метод на стороне клиента точно совпадают с именем или сигнатурой, вызванного метода, вызов завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="4eace-121">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="4eace-122">Убедитесь, что имя метода, который вызывается сервером совпадает имя метода на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-122">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="4eace-123">Кроме того, SignalR создает прокси-сервера концентратора, используя методы стиле Camel, как и соответствующие на языке JavaScript, поэтому вызывается метод `SendMessage` на сервере может быть вызвана `sendMessage` в прокси клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-123">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="4eace-124">Если вы используете `HubName` атрибута в своем коде на стороне сервера, убедитесь, что имя, используемое совпадает имя, используемое для создания концентратора на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-124">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="4eace-125">Если вы не используете `HubName` атрибута, убедитесь, что имя концентратора в клиенте JavaScript стиле Camel, например chatHub вместо ChatHub.</span><span class="sxs-lookup"><span data-stu-id="4eace-125">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="4eace-126">Повторяющееся имя метода на клиенте</span><span class="sxs-lookup"><span data-stu-id="4eace-126">Duplicate method name on client</span></span>

<span data-ttu-id="4eace-127">Убедитесь, что у вас повторяющийся метод на клиенте, отличается только регистром.</span><span class="sxs-lookup"><span data-stu-id="4eace-127">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="4eace-128">Если клиентское приложение содержит метод с именем `sendMessage`, убедитесь, что не существует также метод с именем `SendMessage` также.</span><span class="sxs-lookup"><span data-stu-id="4eace-128">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="4eace-129">Отсутствует средство синтаксического анализа JSON на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="4eace-129">Missing JSON parser on the client</span></span>

<span data-ttu-id="4eace-130">SignalR требуется средство синтаксического анализа JSON для сериализации вызовов между сервером и клиентом.</span><span class="sxs-lookup"><span data-stu-id="4eace-130">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="4eace-131">Если клиент не имеет встроенные средства синтаксического анализа JSON (например, Internet Explorer 7), необходимо включить его в приложении.</span><span class="sxs-lookup"><span data-stu-id="4eace-131">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="4eace-132">Вы можете загрузить средство синтаксического анализа JSON [здесь](http://nuget.org/packages/json2).</span><span class="sxs-lookup"><span data-stu-id="4eace-132">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="4eace-133">Смешивание концентратора и подключение PersistentConnection синтаксиса</span><span class="sxs-lookup"><span data-stu-id="4eace-133">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="4eace-134">SignalR использует две модели взаимодействия: концентраторы и PersistentConnections.</span><span class="sxs-lookup"><span data-stu-id="4eace-134">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="4eace-135">Синтаксис вызова этих двух связи моделей отличается в коде клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-135">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="4eace-136">При добавлении концентратор в коде сервера убедитесь, что весь код клиента использует синтаксис правильный концентратора.</span><span class="sxs-lookup"><span data-stu-id="4eace-136">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="4eace-137">**Клиентский код JavaScript, который создает подключение PersistentConnection в клиент JavaScript**</span><span class="sxs-lookup"><span data-stu-id="4eace-137">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="4eace-138">**Клиентский код JavaScript, который создает прокси-сервер-концентратор в клиент Javascript**</span><span class="sxs-lookup"><span data-stu-id="4eace-138">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="4eace-139">**Серверный код на C#, сопоставляет маршрут подключение PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="4eace-139">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="4eace-140">**Серверный код на C#, сопоставляет маршрут к концентратору или к концентраторам несколькими при наличии нескольких приложений**</span><span class="sxs-lookup"><span data-stu-id="4eace-140">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="4eace-141">Подключения, запустить до добавления подписок</span><span class="sxs-lookup"><span data-stu-id="4eace-141">Connection started before subscriptions are added</span></span>

<span data-ttu-id="4eace-142">Если подключение концентратора запускается до добавления методов, которые могут быть вызваны с сервера прокси-сервер, сообщения не будут приниматься.</span><span class="sxs-lookup"><span data-stu-id="4eace-142">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="4eace-143">Следующий код JavaScript не запустится концентратора должным образом:</span><span class="sxs-lookup"><span data-stu-id="4eace-143">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="4eace-144">**Неверный клиентский код JavaScript, не позволит концентраторов принимать сообщения**</span><span class="sxs-lookup"><span data-stu-id="4eace-144">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="4eace-145">Вместо этого добавьте метод подписки перед вызовом Start:</span><span class="sxs-lookup"><span data-stu-id="4eace-145">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="4eace-146">**Клиентский код JavaScript, который правильно добавляет подписки с концентратором**</span><span class="sxs-lookup"><span data-stu-id="4eace-146">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="4eace-147">Отсутствует имя метода прокси-сервера концентратора</span><span class="sxs-lookup"><span data-stu-id="4eace-147">Missing method name on the hub proxy</span></span>

<span data-ttu-id="4eace-148">Убедитесь, что метод, определенный на сервере подписаться на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-148">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="4eace-149">Несмотря на то, что сервер определяет метод, он по-прежнему должны добавляться в прокси клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-149">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="4eace-150">Добавлены методы прокси-сервер клиента следующим образом (Обратите внимание, что добавляется метод `client` член концентратора не концентратора непосредственно):</span><span class="sxs-lookup"><span data-stu-id="4eace-150">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="4eace-151">**Клиентский код JavaScript, который добавляет методы прокси-сервер концентратора**</span><span class="sxs-lookup"><span data-stu-id="4eace-151">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="4eace-152">Концентратор или методов концентратора не объявлен как Public</span><span class="sxs-lookup"><span data-stu-id="4eace-152">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="4eace-153">Чтобы быть видимым на клиенте, реализации концентратора и методов должны объявляться как `public`.</span><span class="sxs-lookup"><span data-stu-id="4eace-153">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="4eace-154">Доступ к концентратору из другого приложения</span><span class="sxs-lookup"><span data-stu-id="4eace-154">Accessing hub from a different application</span></span>

<span data-ttu-id="4eace-155">Концентраторы SignalR может осуществляться только через приложения, реализующие клиентов SignalR.</span><span class="sxs-lookup"><span data-stu-id="4eace-155">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="4eace-156">SignalR не может взаимодействовать с другими библиотеками взаимодействия (например, SOAP или WCF веб-службы.) Если клиент не SignalR доступен для целевой платформы, не может напрямую обращаться к конечной точке сервера.</span><span class="sxs-lookup"><span data-stu-id="4eace-156">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="4eace-157">Сериализация данных вручную</span><span class="sxs-lookup"><span data-stu-id="4eace-157">Manually serializing data</span></span>

<span data-ttu-id="4eace-158">SignalR будут автоматически использовать JSON для сериализации метод параметров здесь не нужно сделать самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="4eace-158">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="4eace-159">Не выполняется на клиенте в функции OnDisconnected удаленного метода концентратора</span><span class="sxs-lookup"><span data-stu-id="4eace-159">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="4eace-160">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-160">This behavior is by design.</span></span> <span data-ttu-id="4eace-161">Когда `OnDisconnected` — вызове концентратора уже введен `Disconnected` состояние, которое больше нельзя вызывать методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="4eace-161">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="4eace-162">**Серверный код на C#, правильно выполняет код в событии OnDisconnected**</span><span class="sxs-lookup"><span data-stu-id="4eace-162">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a><span data-ttu-id="4eace-163">Достигнут предел для числа подключений</span><span class="sxs-lookup"><span data-stu-id="4eace-163">Connection limit reached</span></span>

<span data-ttu-id="4eace-164">При использовании полной версии IIS в клиентской операционной системе, например Windows 7, ограничений подключения 10.</span><span class="sxs-lookup"><span data-stu-id="4eace-164">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="4eace-165">При использовании клиентской ОС, используйте IIS Express вместо во избежание этого предела.</span><span class="sxs-lookup"><span data-stu-id="4eace-165">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="4eace-166">Не настроено подключение между доменами</span><span class="sxs-lookup"><span data-stu-id="4eace-166">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="4eace-167">Если соединение между доменами (соединение, для которого SignalR URL-адрес не в том же домене, что страница размещения) не настроен правильно, не удается установить подключение без сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4eace-167">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="4eace-168">Сведения о включении обмена данными между доменами см. в разделе [как для установления соединения между доменами](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="4eace-168">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="4eace-169">Подключение с использованием протокола NTLM (Active Directory) не работает в клиент .NET</span><span class="sxs-lookup"><span data-stu-id="4eace-169">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="4eace-170">Подключения в клиентском приложении .NET, которая использует безопасность домена может завершиться ошибкой, если подключение не настроено должным образом.</span><span class="sxs-lookup"><span data-stu-id="4eace-170">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="4eace-171">Задайте свойства соединения, необходимые для использования SignalR в среде домена, следующим образом.</span><span class="sxs-lookup"><span data-stu-id="4eace-171">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="4eace-172">**Клиентский код на C#, реализующий учетных данных соединения**</span><span class="sxs-lookup"><span data-stu-id="4eace-172">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="4eace-173">Другие проблемы с подключением</span><span class="sxs-lookup"><span data-stu-id="4eace-173">Other connection issues</span></span>

<span data-ttu-id="4eace-174">В этом разделе описываются причины и способы устранения конкретных проблем или сообщений об ошибках, возникающих во время соединения.</span><span class="sxs-lookup"><span data-stu-id="4eace-174">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="4eace-175">Ошибка «Start должен вызываться перед отправкой данных»</span><span class="sxs-lookup"><span data-stu-id="4eace-175">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="4eace-176">Эта ошибка часто возникает, если код ссылается на объекты SignalR до начала соединения.</span><span class="sxs-lookup"><span data-stu-id="4eace-176">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="4eace-177">Присоединит для обработчиков и like, будут вызывать методы, определенные на сервере необходимо добавить после создания подключения.</span><span class="sxs-lookup"><span data-stu-id="4eace-177">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="4eace-178">Обратите внимание, что вызов `Start` является асинхронным, поэтому код после вызова может выполняться до его завершения.</span><span class="sxs-lookup"><span data-stu-id="4eace-178">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="4eace-179">Для добавления обработчиков после подключения запуска полностью рекомендуется поместить их в функцию обратного вызова, передаваемый в качестве параметра в метод start:</span><span class="sxs-lookup"><span data-stu-id="4eace-179">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="4eace-180">**Клиентский код JavaScript, который правильно добавляет обработчики событий, которые ссылаются на объекты SignalR**</span><span class="sxs-lookup"><span data-stu-id="4eace-180">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="4eace-181">Эта ошибка также могут появляться, если соединение останавливает время по-прежнему ссылаются объекты SignalR.</span><span class="sxs-lookup"><span data-stu-id="4eace-181">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="4eace-182">«301 перемещен окончательно» или «302 Перемещен временно» ошибка</span><span class="sxs-lookup"><span data-stu-id="4eace-182">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="4eace-183">Эта ошибка может возникать, если проект содержит папку с именем SignalR, в которой будет мешать автоматически создается прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="4eace-183">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="4eace-184">Чтобы избежать этой ошибки, не используйте папку с именем `SignalR` в приложение, или создания прокси на автоматическое включение off.</span><span class="sxs-lookup"><span data-stu-id="4eace-184">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="4eace-185">В разделе [созданного прокси и делает за вас](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="4eace-185">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="4eace-186">Ошибка «403 запрещено» в клиент .NET или Silverlight</span><span class="sxs-lookup"><span data-stu-id="4eace-186">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="4eace-187">Эта ошибка может возникнуть в средах между доменами, где связь между доменами не включена должным образом.</span><span class="sxs-lookup"><span data-stu-id="4eace-187">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="4eace-188">Сведения о включении обмена данными между доменами см. в разделе [как для установления соединения между доменами](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="4eace-188">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="4eace-189">Для установления соединения между доменами в клиент Silverlight, в разделе [соединения между доменами от клиентов Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span><span class="sxs-lookup"><span data-stu-id="4eace-189">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="4eace-190">Ошибка «404 не найдено»</span><span class="sxs-lookup"><span data-stu-id="4eace-190">"404 Not Found" error</span></span>

<span data-ttu-id="4eace-191">Существует несколько причин этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="4eace-191">There are several causes for this issue.</span></span> <span data-ttu-id="4eace-192">Проверьте все указанные ниже:</span><span class="sxs-lookup"><span data-stu-id="4eace-192">Verify all of the following:</span></span>

- <span data-ttu-id="4eace-193">**Ссылка на адресов прокси-сервера концентратора неправильный формат:** Эта ошибка обычно возникает, если ссылку на созданный концентратор прокси-адрес имеет неправильный формат.</span><span class="sxs-lookup"><span data-stu-id="4eace-193">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="4eace-194">Проверьте, правильно становится ссылка на адрес в концентратор.</span><span class="sxs-lookup"><span data-stu-id="4eace-194">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="4eace-195">В разделе [как ссылаться на динамически созданный прокси](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="4eace-195">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="4eace-196">**Добавление маршрутов для приложения, прежде чем добавлять маршрут концентратора:** Если приложение использует другие маршруты, убедитесь, что добавления маршрута первый вызов `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="4eace-196">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapHubs`.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="4eace-197">«500 Внутренняя ошибка сервера»</span><span class="sxs-lookup"><span data-stu-id="4eace-197">"500 Internal Server Error"</span></span>

<span data-ttu-id="4eace-198">Это очень Общая ошибка, которая может иметь широкий спектр причины.</span><span class="sxs-lookup"><span data-stu-id="4eace-198">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="4eace-199">Сведения об ошибке должны появиться в журнале событий сервера, или можно найти по отладку сервера.</span><span class="sxs-lookup"><span data-stu-id="4eace-199">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="4eace-200">Более подробные сведения об ошибке можно получить, включив подробных сообщений об ошибках на сервере.</span><span class="sxs-lookup"><span data-stu-id="4eace-200">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="4eace-201">Дополнительные сведения см. в разделе [способ обработки ошибок в классе концентратора](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span><span class="sxs-lookup"><span data-stu-id="4eace-201">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="4eace-202">«TypeError: &lt;hubType&gt; не определено» ошибки</span><span class="sxs-lookup"><span data-stu-id="4eace-202">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="4eace-203">Эта ошибка возникнет при вызове `MapHubs` не выполняется должным образом.</span><span class="sxs-lookup"><span data-stu-id="4eace-203">This error will result if the call to `MapHubs` is not made properly.</span></span> <span data-ttu-id="4eace-204">В разделе [как зарегистрировать SignalR маршрута и настройте параметры SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="4eace-204">See [How to register the SignalR route and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="4eace-205">JsonSerializationException было обработано пользовательским кодом</span><span class="sxs-lookup"><span data-stu-id="4eace-205">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="4eace-206">Убедитесь, что параметры, отправляемые в методы содержит несериализуемые типов (таких как дескрипторы файлов или подключения к базе данных).</span><span class="sxs-lookup"><span data-stu-id="4eace-206">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="4eace-207">Если необходимо использовать элементы на стороне сервера объекте, но вы не хотите отправить клиенту (или для безопасности или по причинам, сериализации), используйте `JSONIgnore` атрибута.</span><span class="sxs-lookup"><span data-stu-id="4eace-207">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="4eace-208">«Ошибка протокола: неизвестный транспорт» ошибка</span><span class="sxs-lookup"><span data-stu-id="4eace-208">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="4eace-209">Эта ошибка возникает в том случае, если клиент не поддерживает транспортов, которые использует SignalR.</span><span class="sxs-lookup"><span data-stu-id="4eace-209">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="4eace-210">В разделе [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports) сведения, на которой браузеры могут использоваться с SignalR.</span><span class="sxs-lookup"><span data-stu-id="4eace-210">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="4eace-211">«Создание прокси-сервера JavaScript Hub была отключена.»</span><span class="sxs-lookup"><span data-stu-id="4eace-211">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="4eace-212">Эта ошибка возникает, если `DisableJavaScriptProxies` задаются в процессе содержит ссылку на динамически созданный прокси на `signalr/hubs`.</span><span class="sxs-lookup"><span data-stu-id="4eace-212">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="4eace-213">Дополнительные сведения о создании учетной записи-посредника вручную см. в разделе [созданный прокси и делает за вас](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="4eace-213">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="4eace-214">«Идентификатор подключения имеет неправильный формат» или «удостоверение пользователя нельзя изменить во время активного подключения SignalR» ошибка</span><span class="sxs-lookup"><span data-stu-id="4eace-214">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="4eace-215">Эта ошибка может возникать, если используется проверка подлинности и выходе клиента, прежде чем подключение остановлено.</span><span class="sxs-lookup"><span data-stu-id="4eace-215">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="4eace-216">Решение заключается в том, чтобы остановить подключения SignalR до выхода клиента.</span><span class="sxs-lookup"><span data-stu-id="4eace-216">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="4eace-217">«Ошибка неперехваченное: SignalR: не удалось найти jQuery.</span><span class="sxs-lookup"><span data-stu-id="4eace-217">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="4eace-218">Ошибка, убедитесь, что существует jQuery перед SignalR.js файл»</span><span class="sxs-lookup"><span data-stu-id="4eace-218">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="4eace-219">SignalR JavaScript для клиента требуется jQuery для запуска.</span><span class="sxs-lookup"><span data-stu-id="4eace-219">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="4eace-220">Проверьте правильность пути, используемого, а также ссылку на jQuery предшествует ссылку на SignalR справочной информации для jQuery.</span><span class="sxs-lookup"><span data-stu-id="4eace-220">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="4eace-221">«Неперехваченное TypeError: не удалось считать свойство "&lt;свойство&gt;" не определено для» ошибка</span><span class="sxs-lookup"><span data-stu-id="4eace-221">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="4eace-222">Эта ошибка возникает из отсутствия jQuery или концентраторов прокси-сервера, правильность ссылок.</span><span class="sxs-lookup"><span data-stu-id="4eace-222">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="4eace-223">Убедитесь, что справочной информации для jQuery и прокси-концентраторы правильность пути, используемого, а также ссылку на jQuery предшествует ссылку на концентраторы прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="4eace-223">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="4eace-224">Справочник по умолчанию концентраторов прокси-сервер должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4eace-224">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="4eace-225">**Клиентский код HTML, правильно ссылается на учетную запись-посредник концентраторы**</span><span class="sxs-lookup"><span data-stu-id="4eace-225">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="4eace-226">Ошибка «RuntimeBinderException было обработано пользовательским кодом»</span><span class="sxs-lookup"><span data-stu-id="4eace-226">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="4eace-227">Эта ошибка может возникать при неверные перегруженный `Hub.On` используется.</span><span class="sxs-lookup"><span data-stu-id="4eace-227">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="4eace-228">Если метод имеет возвращаемое значение, тип возвращаемого значения указывается как параметр универсального типа:</span><span class="sxs-lookup"><span data-stu-id="4eace-228">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="4eace-229">**Метод, определенный на клиенте (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="4eace-229">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="4eace-230">Идентификатор соединения не согласована, или разрыва соединения между загрузке страницы</span><span class="sxs-lookup"><span data-stu-id="4eace-230">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="4eace-231">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-231">This behavior is by design.</span></span> <span data-ttu-id="4eace-232">Поскольку объект концентратора размещается в объект страницы, концентратор уничтожается при обновлении страницы.</span><span class="sxs-lookup"><span data-stu-id="4eace-232">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="4eace-233">Приложению многостраничную для поддержки связи между пользователями и идентификаторов подключений, чтобы они были согласованы между загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="4eace-233">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="4eace-234">Идентификаторов подключений, которые могут храниться на сервере, либо `ConcurrentDictionary` объекта или базы данных.</span><span class="sxs-lookup"><span data-stu-id="4eace-234">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="4eace-235">Ошибка «Значение не может быть null»</span><span class="sxs-lookup"><span data-stu-id="4eace-235">"Value cannot be null" error</span></span>

<span data-ttu-id="4eace-236">Методы на стороне сервера с необязательными параметрами в настоящее время не поддерживаются; Если необязательный параметр опущен, метод завершится с ошибкой.</span><span class="sxs-lookup"><span data-stu-id="4eace-236">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="4eace-237">Дополнительные сведения см. в разделе [необязательные параметры](https://github.com/SignalR/SignalR/issues/324).</span><span class="sxs-lookup"><span data-stu-id="4eace-237">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="4eace-238">«Firefox не удается установить подключение к серверу через &lt;адрес&gt;«ошибка в Firebug</span><span class="sxs-lookup"><span data-stu-id="4eace-238">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="4eace-239">Это сообщение об ошибке может отображаться в Firebug, если происходит сбой согласования транспорта WebSocket и вместо него используется другой транспорт.</span><span class="sxs-lookup"><span data-stu-id="4eace-239">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="4eace-240">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-240">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="4eace-241">Ошибка «удаленный сертификат недействителен согласно процедуры проверки» в клиентском приложении .NET</span><span class="sxs-lookup"><span data-stu-id="4eace-241">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="4eace-242">Если сервер требует пользовательских клиентских сертификатов, затем можно добавить подключение x509certificate, перед выполнением запроса.</span><span class="sxs-lookup"><span data-stu-id="4eace-242">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="4eace-243">Добавить сертификат для соединения с помощью `Connection.AddClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="4eace-243">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="4eace-244">Удаляет соединение после проверки подлинности время ожидания</span><span class="sxs-lookup"><span data-stu-id="4eace-244">Connection drops after authentication times out</span></span>

<span data-ttu-id="4eace-245">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-245">This behavior is by design.</span></span> <span data-ttu-id="4eace-246">Учетные данные проверки подлинности нельзя изменить, если подключение активно; Чтобы обновить учетные данные, соединение необходимо остановить и перезапустить.</span><span class="sxs-lookup"><span data-stu-id="4eace-246">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="4eace-247">OnConnected вызывается два раза в том случае, когда с помощью jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="4eace-247">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="4eace-248">jQuery Mobile `initializePage` функция принудительно скрипты на каждой странице, чтобы выполнить повторно, тем самым создавая второе подключение.</span><span class="sxs-lookup"><span data-stu-id="4eace-248">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="4eace-249">Включить решения этой проблемы:</span><span class="sxs-lookup"><span data-stu-id="4eace-249">Solutions for this issue include:</span></span>

- <span data-ttu-id="4eace-250">Включайте ссылку на jQuery Mobile, прежде чем файл JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4eace-250">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="4eace-251">Отключить `initializePage` функции, задав `$.mobile.autoInitializePage = false`.</span><span class="sxs-lookup"><span data-stu-id="4eace-251">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="4eace-252">Подождите, пока страница для завершения инициализации перед запуском соединения.</span><span class="sxs-lookup"><span data-stu-id="4eace-252">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="4eace-253">Сообщения задержаны в приложениях Silverlight, с помощью отправки событий сервера</span><span class="sxs-lookup"><span data-stu-id="4eace-253">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="4eace-254">Сообщения задержаны об отправке с помощью сервера события на Silverlight.</span><span class="sxs-lookup"><span data-stu-id="4eace-254">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="4eace-255">Чтобы принудительно продолжительный опрос для использования вместо этого, используйте следующие параметры при запуске подключения.</span><span class="sxs-lookup"><span data-stu-id="4eace-255">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="4eace-256">С помощью «Разрешение Denied» навсегда фрейма протокола</span><span class="sxs-lookup"><span data-stu-id="4eace-256">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="4eace-257">Это известная проблема, описано [здесь](https://github.com/SignalR/SignalR/issues/1963).</span><span class="sxs-lookup"><span data-stu-id="4eace-257">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="4eace-258">Эта проблема может возникать, с помощью последней библиотеки JQuery; Это решение подходит для возврата к предыдущей версии приложения для JQuery 1.8.2.</span><span class="sxs-lookup"><span data-stu-id="4eace-258">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="4eace-259">Ошибки компиляции и на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="4eace-259">Compilation and server-side errors</span></span>

 <span data-ttu-id="4eace-260">В следующем разделе приведены возможные решения для компилятора и ошибки времени выполнения стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="4eace-260">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="4eace-261">Ссылка на экземпляр концентратора имеет значение null</span><span class="sxs-lookup"><span data-stu-id="4eace-261">Reference to Hub instance is null</span></span>

<span data-ttu-id="4eace-262">Так как для каждого соединения, создается экземпляр концентратора невозможно создать экземпляр концентратора в коде самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="4eace-262">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="4eace-263">Для вызова методов в концентратор из за пределами самого концентратора, в разделе [как вызывать методы клиента групп и управление ими из вне класса концентратора](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) способ получить ссылку на контекст концентратора.</span><span class="sxs-lookup"><span data-stu-id="4eace-263">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="4eace-264">HTTPContext.Current.Session имеет значение null</span><span class="sxs-lookup"><span data-stu-id="4eace-264">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="4eace-265">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-265">This behavior is by design.</span></span> <span data-ttu-id="4eace-266">SignalR не поддерживает состояние сеанса ASP.NET, так как Включение состояния сеанса будет нарушена дуплексного обмена сообщениями.</span><span class="sxs-lookup"><span data-stu-id="4eace-266">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="4eace-267">Не подходящий метод для переопределения</span><span class="sxs-lookup"><span data-stu-id="4eace-267">No suitable method to override</span></span>

<span data-ttu-id="4eace-268">Появится это сообщение об ошибке при использовании кода из документацию к предыдущим версиям или блогов.</span><span class="sxs-lookup"><span data-stu-id="4eace-268">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="4eace-269">Убедитесь, что вы не ссылаются на имена методов, которые были изменены или устаревшие (такие как `OnConnectedAsync`).</span><span class="sxs-lookup"><span data-stu-id="4eace-269">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="4eace-270">HostContextExtensions.WebSocketServerUrl имеет значение null</span><span class="sxs-lookup"><span data-stu-id="4eace-270">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="4eace-271">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-271">This behavior is by design.</span></span> <span data-ttu-id="4eace-272">Данный член является устаревшим и не должны использоваться.</span><span class="sxs-lookup"><span data-stu-id="4eace-272">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="4eace-273">Ошибка «маршрут с именем «signalr.hubs» уже находится в коллекции маршрутов»</span><span class="sxs-lookup"><span data-stu-id="4eace-273">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="4eace-274">Эта ошибка возникает, если `MapHubs` вызывается дважды для приложения.</span><span class="sxs-lookup"><span data-stu-id="4eace-274">This error will be seen if `MapHubs` is called twice by your application.</span></span> <span data-ttu-id="4eace-275">Некоторые приложения вызывают пример `MapHubs` непосредственно в файле глобального приложения; другим пользователям в вызове в класс-оболочку.</span><span class="sxs-lookup"><span data-stu-id="4eace-275">Some example applications call `MapHubs` directly in the global application file; others make the call in a wrapper class.</span></span> <span data-ttu-id="4eace-276">Убедитесь, что приложение не оба.</span><span class="sxs-lookup"><span data-stu-id="4eace-276">Ensure that your application does not do both.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="4eace-277">Проблемы Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4eace-277">Visual Studio issues</span></span>

<span data-ttu-id="4eace-278">В этом разделе описываются проблем, возникающих в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4eace-278">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="4eace-279">Узел документов скрипта не отображается в обозревателе решений</span><span class="sxs-lookup"><span data-stu-id="4eace-279">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="4eace-280">Некоторые из наших учебниках дать вам ссылки на узел «Документы скрипта» в обозревателе решений во время отладки.</span><span class="sxs-lookup"><span data-stu-id="4eace-280">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="4eace-281">Этот узел произведенное отладчик JavaScript и отображается только при отладке клиенты браузера в Internet Explorer; узел не будет отображаться при использовании Chrome или Firefox.</span><span class="sxs-lookup"><span data-stu-id="4eace-281">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="4eace-282">Отладчик JavaScript также не будет выполняться, если выполняется другой отладчик клиента, такие как отладчик Silverlight.</span><span class="sxs-lookup"><span data-stu-id="4eace-282">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="4eace-283">SignalR не работает в Visual Studio 2008 или более ранней версии</span><span class="sxs-lookup"><span data-stu-id="4eace-283">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="4eace-284">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="4eace-284">This behavior is by design.</span></span> <span data-ttu-id="4eace-285">SignalR требуется платформа .NET Framework 4 или более поздней версии; для этого необходимо разработать SignalR приложений в Visual Studio 2010 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="4eace-285">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="4eace-286">Проблем IIS</span><span class="sxs-lookup"><span data-stu-id="4eace-286">IIS issues</span></span>

<span data-ttu-id="4eace-287">Этот раздел содержит проблем со службами IIS.</span><span class="sxs-lookup"><span data-stu-id="4eace-287">This section contains issues with Internet Information Services.</span></span>

### <a name="web-site-crashes-after-maphubs-call"></a><span data-ttu-id="4eace-288">После вызова MapHubs аварийно завершает работу веб-сайта</span><span class="sxs-lookup"><span data-stu-id="4eace-288">Web site crashes after MapHubs call</span></span>

<span data-ttu-id="4eace-289">Эта проблема исправлена в последней версии SignalR.</span><span class="sxs-lookup"><span data-stu-id="4eace-289">This issue has been fixed in the latest version of SignalR.</span></span> <span data-ttu-id="4eace-290">Убедитесь, что вы используете последней версии SignalR путем обновления установки с помощью NuGet.</span><span class="sxs-lookup"><span data-stu-id="4eace-290">Verify that you are using the latest released version of SignalR by updating your installation using NuGet.</span></span>

<a id="azure"></a>

## <a name="azure-issues"></a><span data-ttu-id="4eace-291">Проблемы Azure</span><span class="sxs-lookup"><span data-stu-id="4eace-291">Azure issues</span></span>

<span data-ttu-id="4eace-292">Этот раздел содержит проблемы, связанные с Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4eace-292">This section contains issues with Microsoft Azure.</span></span>

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="4eace-293">Не были получены сообщения через Azure объединительной платы после изменения раздела имена</span><span class="sxs-lookup"><span data-stu-id="4eace-293">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="4eace-294">Разделы, используемые Azure объединительной платы не предназначены для настройки пользователя.</span><span class="sxs-lookup"><span data-stu-id="4eace-294">The topics used by the Azure backplane are not intended to be user-configurable.</span></span>
