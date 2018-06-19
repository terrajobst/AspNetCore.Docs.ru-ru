---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Включение трассировки SignalR | Документы Microsoft
author: tfitzmac
description: Этот документ описывает, как включить и настроить трассировку для SignalR серверов и клиентов. Трассировка позволяет просматривать диагностические сведения о событиях...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28032822"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="adc07-104">Включение трассировки SignalR</span><span class="sxs-lookup"><span data-stu-id="adc07-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="adc07-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="adc07-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="adc07-106">Этот документ описывает, как включить и настроить трассировку для SignalR серверов и клиентов.</span><span class="sxs-lookup"><span data-stu-id="adc07-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="adc07-107">Трассировка позволяет просматривать диагностические сведения о событиях в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="adc07-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="adc07-108">В этом разделе изначально был записан с Патрик Флетчера.</span><span class="sxs-lookup"><span data-stu-id="adc07-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="adc07-109">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="adc07-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="adc07-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="adc07-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="adc07-111">.NET Framework 4,5</span><span class="sxs-lookup"><span data-stu-id="adc07-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="adc07-112">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="adc07-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="adc07-113">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="adc07-113">Questions and comments</span></span>
> 
> <span data-ttu-id="adc07-114">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="adc07-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="adc07-115">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="adc07-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="adc07-116">При включенной трассировке приложения SignalR создает записи журнала для событий.</span><span class="sxs-lookup"><span data-stu-id="adc07-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="adc07-117">Можно вести журнал событий от клиента и сервера.</span><span class="sxs-lookup"><span data-stu-id="adc07-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="adc07-118">Трассировка выполняется на журналы подключение к серверу, поставщик горизонтального масштабирования и события шины сообщений.</span><span class="sxs-lookup"><span data-stu-id="adc07-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="adc07-119">Трассировка выполняется на события подключения журналы на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="adc07-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="adc07-120">В SignalR 2.1 и более поздних версиях трассировки на клиенте записывает полное содержимое сообщения вызова концентратора.</span><span class="sxs-lookup"><span data-stu-id="adc07-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="adc07-121">Описание</span><span class="sxs-lookup"><span data-stu-id="adc07-121">Contents</span></span>

- [<span data-ttu-id="adc07-122">Включение трассировки на сервере</span><span class="sxs-lookup"><span data-stu-id="adc07-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="adc07-123">Ведение журнала событий сервера в текстовые файлы</span><span class="sxs-lookup"><span data-stu-id="adc07-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="adc07-124">Ведение журнала событий сервера в журнал событий</span><span class="sxs-lookup"><span data-stu-id="adc07-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="adc07-125">Включение трассировки клиента .NET (приложения для Windows Desktop)</span><span class="sxs-lookup"><span data-stu-id="adc07-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="adc07-126">Ведение журнала событий клиента рабочего стола на консоль</span><span class="sxs-lookup"><span data-stu-id="adc07-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="adc07-127">Ведение журнала событий клиента рабочего стола в текстовый файл</span><span class="sxs-lookup"><span data-stu-id="adc07-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="adc07-128">Включение трассировки в клиентах Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="adc07-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="adc07-129">Ведение журнала событий клиента Windows Phone к пользовательскому Интерфейсу</span><span class="sxs-lookup"><span data-stu-id="adc07-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="adc07-130">Ведение журнала событий клиента Windows Phone в консоли отладки</span><span class="sxs-lookup"><span data-stu-id="adc07-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="adc07-131">Включение трассировки клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="adc07-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="adc07-132">Включение трассировки на сервере</span><span class="sxs-lookup"><span data-stu-id="adc07-132">Enabling tracing on the server</span></span>

<span data-ttu-id="adc07-133">Для включения трассировки на сервере в файле конфигурации приложения (App.config или Web.config в зависимости от типа проекта.) Укажите, какие категории событий следует заносить в журнал.</span><span class="sxs-lookup"><span data-stu-id="adc07-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="adc07-134">В файле конфигурации также укажите, требуется ли в журнал события в текстовый файл, журнал событий Windows или имя пользовательского журнала, используя реализацию [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="adc07-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="adc07-135">Категории событий сервера включают следующие типы сообщений:</span><span class="sxs-lookup"><span data-stu-id="adc07-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="adc07-136">Исходный код</span><span class="sxs-lookup"><span data-stu-id="adc07-136">Source</span></span> | <span data-ttu-id="adc07-137">Сообщения</span><span class="sxs-lookup"><span data-stu-id="adc07-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="adc07-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="adc07-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="adc07-139">Установки поставщика SQL канала сообщений горизонтального масштабирования, операции в базе данных, ошибки и события времени ожидания</span><span class="sxs-lookup"><span data-stu-id="adc07-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="adc07-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="adc07-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="adc07-141">Создание раздела поставщика горизонтального масштабирования шины службы и подписки, ошибок и событий обмена сообщениями</span><span class="sxs-lookup"><span data-stu-id="adc07-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="adc07-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="adc07-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="adc07-143">События подключения, отключения и ошибка поставщика горизонтального масштабирования redis</span><span class="sxs-lookup"><span data-stu-id="adc07-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="adc07-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="adc07-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="adc07-145">Событий сообщений горизонтального масштабирования</span><span class="sxs-lookup"><span data-stu-id="adc07-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="adc07-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="adc07-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="adc07-147">События подключения, отключения, обмен сообщениями и ошибка транспорт WebSocket</span><span class="sxs-lookup"><span data-stu-id="adc07-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="adc07-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="adc07-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="adc07-149">ServerSentEvents транспорта события подключения, отключения, обмен сообщениями и ошибки</span><span class="sxs-lookup"><span data-stu-id="adc07-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="adc07-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="adc07-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="adc07-151">События ForeverFrame транспортного подключения, отключения, обмен сообщениями и ошибки</span><span class="sxs-lookup"><span data-stu-id="adc07-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="adc07-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="adc07-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="adc07-153">События LongPolling транспортного подключения, отключения, обмен сообщениями и ошибки</span><span class="sxs-lookup"><span data-stu-id="adc07-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="adc07-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="adc07-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="adc07-155">События подключения, отключения и keepalive транспорта</span><span class="sxs-lookup"><span data-stu-id="adc07-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="adc07-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="adc07-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="adc07-157">Концентратор событий обнаружения</span><span class="sxs-lookup"><span data-stu-id="adc07-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="adc07-158">Ведение журнала событий сервера в текстовые файлы</span><span class="sxs-lookup"><span data-stu-id="adc07-158">Logging server events to text files</span></span>

<span data-ttu-id="adc07-159">Следующий код показывает, как включить трассировку для каждой категории событий.</span><span class="sxs-lookup"><span data-stu-id="adc07-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="adc07-160">В этом примере приложение настраивается для регистрации событий в текстовые файлы.</span><span class="sxs-lookup"><span data-stu-id="adc07-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="adc07-161">**XML-код сервера для включения трассировки**</span><span class="sxs-lookup"><span data-stu-id="adc07-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="adc07-162">В представленном выше коде `SignalRSwitch` запись указывает [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) используются для отправки в указанном журнале событий.</span><span class="sxs-lookup"><span data-stu-id="adc07-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="adc07-163">В этом случае оно равно `Verbose` регистрируются означает, что все сообщения отладки и трассировки.</span><span class="sxs-lookup"><span data-stu-id="adc07-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="adc07-164">Ниже показано из записи `transports.log.txt` файл с помощью указанного выше файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="adc07-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="adc07-165">Он иллюстрирует создание нового соединения, удаленное подключение и события пульс транспорта.</span><span class="sxs-lookup"><span data-stu-id="adc07-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="adc07-166">Ведение журнала событий сервера в журнал событий</span><span class="sxs-lookup"><span data-stu-id="adc07-166">Logging server events to the event log</span></span>

<span data-ttu-id="adc07-167">Чтобы записывать события в журнал событий, а не текстовый файл, изменить значения для записи в `sharedListeners` узла.</span><span class="sxs-lookup"><span data-stu-id="adc07-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="adc07-168">Ниже показано, как в журнал событий сервера в журнал событий:</span><span class="sxs-lookup"><span data-stu-id="adc07-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="adc07-169">**XML-код сервера для ведения журнала событий в журнал событий**</span><span class="sxs-lookup"><span data-stu-id="adc07-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="adc07-170">События регистрируются в журнале приложений и доступны через средство просмотра событий, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="adc07-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Средство просмотра событий отображаются журналы SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="adc07-172">При использовании журнала событий, установить **TraceLevel** для **ошибка** для сохранения управляемых число сообщений.</span><span class="sxs-lookup"><span data-stu-id="adc07-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="adc07-173">Включение трассировки клиента .NET (приложения для Windows Desktop)</span><span class="sxs-lookup"><span data-stu-id="adc07-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="adc07-174">Клиент .NET можно вести журнал в консоль, текстовый файл, или пользовательский журнал, используя реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="adc07-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="adc07-175">Включение ведения журнала в клиент .NET, задайте соединение с `TraceLevel` свойства [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) значение и `TraceWriter` свойство допустимому [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="adc07-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="adc07-176">Ведение журнала событий клиента рабочего стола на консоль</span><span class="sxs-lookup"><span data-stu-id="adc07-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="adc07-177">Следующий код C# показано, как регистрация событий в клиент .NET на консоль:</span><span class="sxs-lookup"><span data-stu-id="adc07-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="adc07-178">Ведение журнала событий клиента рабочего стола в текстовый файл</span><span class="sxs-lookup"><span data-stu-id="adc07-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="adc07-179">Следующий код C# показано, как регистрация событий в клиент .NET в текстовый файл:</span><span class="sxs-lookup"><span data-stu-id="adc07-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="adc07-180">Ниже показано из записи `ClientLog.txt` файл с помощью указанного выше файла конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="adc07-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="adc07-181">Показывает клиент, подключающийся к серверу, и вызывается вызов клиентского метода концентратора `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="adc07-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="adc07-182">Включение трассировки в клиентах Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="adc07-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="adc07-183">SignalR приложения для приложений Windows Phone используют один и тот же самый клиент .NET как классические приложения, но [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) и записи в файл с [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) недоступны.</span><span class="sxs-lookup"><span data-stu-id="adc07-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="adc07-184">Вместо этого необходимо создать пользовательскую реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) для трассировки.</span><span class="sxs-lookup"><span data-stu-id="adc07-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="adc07-185">Ведение журнала событий клиента Windows Phone к пользовательскому Интерфейсу</span><span class="sxs-lookup"><span data-stu-id="adc07-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="adc07-186">[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) представлен образец Windows Phone, записывает выходные данные трассировки в [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) с использованием пользовательской [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) реализация вызван `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="adc07-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="adc07-187">Этот класс можно найти в **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** проекта.</span><span class="sxs-lookup"><span data-stu-id="adc07-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="adc07-188">При создании экземпляра `TextBlockWriter`, передайте в текущем [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)и [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) где создаст [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) для трассировки выходные данные:</span><span class="sxs-lookup"><span data-stu-id="adc07-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="adc07-189">Выходные данные трассировки затем записываются в новый [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) в [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) передается в:</span><span class="sxs-lookup"><span data-stu-id="adc07-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="adc07-190">Ведение журнала событий клиента Windows Phone в консоли отладки</span><span class="sxs-lookup"><span data-stu-id="adc07-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="adc07-191">Для отправки выходных данных консоли отладки, а не пользовательский Интерфейс, создайте реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , записывает в окно отладки и назначьте его подключение к [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) свойства:</span><span class="sxs-lookup"><span data-stu-id="adc07-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="adc07-192">Данные трассировки записываются в окно отладки в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="adc07-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="adc07-193">Включение трассировки клиента JavaScript</span><span class="sxs-lookup"><span data-stu-id="adc07-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="adc07-194">Чтобы включить ведение журнала клиентского соединения, установите `logging` свойство в объекте подключения перед вызовом метода `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="adc07-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="adc07-195">**Клиентский код JavaScript для включения трассировки на консоль браузера (с созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="adc07-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="adc07-196">**Клиентский код JavaScript для включения трассировки на консоль браузера (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="adc07-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="adc07-197">Если трассировка включена, клиент JavaScript записывает события в консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="adc07-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="adc07-198">Для доступа к консоли браузера, в разделе [мониторинг транспортов](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="adc07-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="adc07-199">На следующем снимке экрана показано клиента SignalR JavaScript с включенной трассировкой.</span><span class="sxs-lookup"><span data-stu-id="adc07-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="adc07-200">В консоли браузера показано соединение и вызова события концентратора.</span><span class="sxs-lookup"><span data-stu-id="adc07-200">It shows connection and hub invocation events in the browser console:</span></span>

![События трассировки SignalR в консоли браузера](enabling-signalr-tracing/_static/image4.png)
