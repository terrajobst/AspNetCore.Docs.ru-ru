---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Включение трассировки SignalR | Документация Майкрософт
author: tfitzmac
description: В этом документе описывается, как включить и настроить трассировку для SignalR серверов и клиентов. Трассировка позволяет просмотреть диагностические сведения о событиях...
ms.author: riande
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 89b27267bec5edb0692fe75061d08b4688df5a8c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912071"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="f7b37-104">Включение трассировки SignalR</span><span class="sxs-lookup"><span data-stu-id="f7b37-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="f7b37-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f7b37-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f7b37-106">В этом документе описывается, как включить и настроить трассировку для SignalR серверов и клиентов.</span><span class="sxs-lookup"><span data-stu-id="f7b37-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="f7b37-107">Трассировка позволяет просмотреть диагностические сведения о событиях в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="f7b37-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="f7b37-108">В этом разделе был первоначально написан Майклом Патрик Флетчера.</span><span class="sxs-lookup"><span data-stu-id="f7b37-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f7b37-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="f7b37-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="f7b37-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f7b37-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f7b37-111">.NET Framework 4,5</span><span class="sxs-lookup"><span data-stu-id="f7b37-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="f7b37-112">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="f7b37-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f7b37-113">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="f7b37-113">Questions and comments</span></span>
>
> <span data-ttu-id="f7b37-114">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="f7b37-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f7b37-115">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f7b37-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f7b37-116">Если трассировка включена, приложение SignalR создает записи журнала для событий.</span><span class="sxs-lookup"><span data-stu-id="f7b37-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="f7b37-117">Можно записывать события из клиента и сервера.</span><span class="sxs-lookup"><span data-stu-id="f7b37-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="f7b37-118">Трассировка на соединение с сервером журналы, поставщике горизонтального масштабирования и события шины сообщений.</span><span class="sxs-lookup"><span data-stu-id="f7b37-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="f7b37-119">Трассировка для событий подключения журналов клиента.</span><span class="sxs-lookup"><span data-stu-id="f7b37-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="f7b37-120">В SignalR 2.1 и более поздних версиях трассировки на клиенте регистрирует полное содержимое сообщения вызова концентратора.</span><span class="sxs-lookup"><span data-stu-id="f7b37-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="f7b37-121">Описание</span><span class="sxs-lookup"><span data-stu-id="f7b37-121">Contents</span></span>

- [<span data-ttu-id="f7b37-122">Включение трассировки на сервере</span><span class="sxs-lookup"><span data-stu-id="f7b37-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="f7b37-123">Ведение журнала событий сервера для текстовых файлов</span><span class="sxs-lookup"><span data-stu-id="f7b37-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="f7b37-124">Ведение журнала событий сервера в журнал событий</span><span class="sxs-lookup"><span data-stu-id="f7b37-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="f7b37-125">Включение трассировки клиента .NET (приложения для Windows Desktop)</span><span class="sxs-lookup"><span data-stu-id="f7b37-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="f7b37-126">Ведение журнала событий клиента рабочего стола на консоль</span><span class="sxs-lookup"><span data-stu-id="f7b37-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="f7b37-127">Ведение журнала событий клиента рабочего стола в текстовый файл</span><span class="sxs-lookup"><span data-stu-id="f7b37-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="f7b37-128">Включение трассировки в клиенты Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="f7b37-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="f7b37-129">Ведение журнала событий клиента Windows Phone в пользовательский интерфейс</span><span class="sxs-lookup"><span data-stu-id="f7b37-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="f7b37-130">Ведение журнала событий клиента Windows Phone в консоль отладки</span><span class="sxs-lookup"><span data-stu-id="f7b37-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="f7b37-131">Включение трассировки в клиенте JavaScript</span><span class="sxs-lookup"><span data-stu-id="f7b37-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="f7b37-132">Включение трассировки на сервере</span><span class="sxs-lookup"><span data-stu-id="f7b37-132">Enabling tracing on the server</span></span>

<span data-ttu-id="f7b37-133">Трассировка выполняется на сервере в файле конфигурации приложения (App.config или Web.config в зависимости от типа проекта.) Можно указать, какие категории событий будут использоваться для входа.</span><span class="sxs-lookup"><span data-stu-id="f7b37-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="f7b37-134">В файле конфигурации, вы также укажите, следует ли регистрировать события в текстовый файл, журнал событий Windows или пользовательский журнал, используя реализацию [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="f7b37-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="f7b37-135">Категории событий сервера включают следующие типы сообщений:</span><span class="sxs-lookup"><span data-stu-id="f7b37-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="f7b37-136">Исходный код</span><span class="sxs-lookup"><span data-stu-id="f7b37-136">Source</span></span> | <span data-ttu-id="f7b37-137">Сообщения</span><span class="sxs-lookup"><span data-stu-id="f7b37-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="f7b37-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="f7b37-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="f7b37-139">Установки поставщика SQL канала сообщений горизонтального масштабирования, операция базы данных, ошибки и события времени ожидания</span><span class="sxs-lookup"><span data-stu-id="f7b37-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="f7b37-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="f7b37-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="f7b37-141">Создание раздела поставщика для горизонтального масштабирования службы шины и подписки, ошибок и событий сообщений</span><span class="sxs-lookup"><span data-stu-id="f7b37-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="f7b37-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="f7b37-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="f7b37-143">События подключения, отключения и ошибка поставщика горизонтального масштабирования redis</span><span class="sxs-lookup"><span data-stu-id="f7b37-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="f7b37-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="f7b37-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="f7b37-145">Событий сообщений горизонтального масштабирования</span><span class="sxs-lookup"><span data-stu-id="f7b37-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="f7b37-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="f7b37-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="f7b37-147">События подключения, отключения, обмен сообщениями и ошибка транспорт WebSocket</span><span class="sxs-lookup"><span data-stu-id="f7b37-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f7b37-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="f7b37-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="f7b37-149">ServerSentEvents транспорта подключение, отключение, обмен сообщениями и ошибка события</span><span class="sxs-lookup"><span data-stu-id="f7b37-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f7b37-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="f7b37-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="f7b37-151">Транспортные ForeverFrame подключение, отключение, обмен сообщениями и ошибка события</span><span class="sxs-lookup"><span data-stu-id="f7b37-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f7b37-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="f7b37-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="f7b37-153">Транспортные LongPolling подключение, отключение, обмен сообщениями и ошибка события</span><span class="sxs-lookup"><span data-stu-id="f7b37-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f7b37-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="f7b37-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="f7b37-155">События подключения, отключения и keepalive транспорта</span><span class="sxs-lookup"><span data-stu-id="f7b37-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="f7b37-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="f7b37-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="f7b37-157">Концентратор событий обнаружения</span><span class="sxs-lookup"><span data-stu-id="f7b37-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="f7b37-158">Ведение журнала событий сервера для текстовых файлов</span><span class="sxs-lookup"><span data-stu-id="f7b37-158">Logging server events to text files</span></span>

<span data-ttu-id="f7b37-159">Ниже показано, как включить трассировку для каждой категории событий.</span><span class="sxs-lookup"><span data-stu-id="f7b37-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="f7b37-160">Этот пример настраивает приложение для регистрации событий в текстовый файл.</span><span class="sxs-lookup"><span data-stu-id="f7b37-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="f7b37-161">**XML-код сервера для включения трассировки**</span><span class="sxs-lookup"><span data-stu-id="f7b37-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="f7b37-162">В приведенном выше коде в `SignalRSwitch` запись указывает [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) для событий, отправленных в указанном журнале.</span><span class="sxs-lookup"><span data-stu-id="f7b37-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="f7b37-163">В этом случае оно имеет значение `Verbose` регистрируются означающее, что все сообщения отладки и трассировки.</span><span class="sxs-lookup"><span data-stu-id="f7b37-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="f7b37-164">В следующем примере вывод записей из `transports.log.txt` файла для приложения с помощью указанного выше файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f7b37-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="f7b37-165">Он иллюстрирует создание нового подключения, удаленные подключения и события пульса транспорта.</span><span class="sxs-lookup"><span data-stu-id="f7b37-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="f7b37-166">Ведение журнала событий сервера в журнал событий</span><span class="sxs-lookup"><span data-stu-id="f7b37-166">Logging server events to the event log</span></span>

<span data-ttu-id="f7b37-167">Чтобы регистрировать события в журнале событий, а не в текстовый файл, измените значения записей в `sharedListeners` узла.</span><span class="sxs-lookup"><span data-stu-id="f7b37-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="f7b37-168">Ниже показано, как регистрировать события сервера в журнале событий:</span><span class="sxs-lookup"><span data-stu-id="f7b37-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="f7b37-169">**XML-код сервера для регистрации событий в журнале событий**</span><span class="sxs-lookup"><span data-stu-id="f7b37-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="f7b37-170">События регистрируются в журнале приложений и доступны через средство просмотра событий, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="f7b37-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Средство просмотра событий, отображаться журналы SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="f7b37-172">При использовании в журнал событий, установить **TraceLevel** для **ошибка** следует управляемое количество сообщений.</span><span class="sxs-lookup"><span data-stu-id="f7b37-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="f7b37-173">Включение трассировки клиента .NET (приложения для Windows Desktop)</span><span class="sxs-lookup"><span data-stu-id="f7b37-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="f7b37-174">Клиент .NET можно регистрировать события в консоль, текстовый файл, или пользовательский журнал, используя реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7b37-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="f7b37-175">Чтобы включить ведение журнала в клиенте .NET, задайте соединение с `TraceLevel` свойства [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) значение и `TraceWriter` свойство на допустимый [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) экземпляра.</span><span class="sxs-lookup"><span data-stu-id="f7b37-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="f7b37-176">Ведение журнала событий клиента рабочего стола на консоль</span><span class="sxs-lookup"><span data-stu-id="f7b37-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="f7b37-177">Следующий код C# показано, как регистрировать события в клиенте .NET на консоль:</span><span class="sxs-lookup"><span data-stu-id="f7b37-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="f7b37-178">Ведение журнала событий клиента рабочего стола в текстовый файл</span><span class="sxs-lookup"><span data-stu-id="f7b37-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="f7b37-179">Следующий код C# показано, как регистрировать события в клиенте .NET в текстовый файл:</span><span class="sxs-lookup"><span data-stu-id="f7b37-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="f7b37-180">В следующем примере вывод записей из `ClientLog.txt` файла для приложения с помощью указанного выше файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f7b37-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="f7b37-181">В нем показано клиент, подключающийся к серверу, и вызов клиентского метода концентратора вызывается `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="f7b37-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="f7b37-182">Включение трассировки в клиенты Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="f7b37-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="f7b37-183">SignalR приложения для приложений Windows Phone используют один и тот же клиент .NET как классические приложения, но [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) и запись в файл с [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) недоступны.</span><span class="sxs-lookup"><span data-stu-id="f7b37-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="f7b37-184">Вместо этого необходимо создать пользовательскую реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) для трассировки.</span><span class="sxs-lookup"><span data-stu-id="f7b37-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="f7b37-185">Ведение журнала событий клиента Windows Phone в пользовательский интерфейс</span><span class="sxs-lookup"><span data-stu-id="f7b37-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="f7b37-186">[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) имеется пример Windows Phone, записывает выходные данные трассировки [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) с использованием пользовательской [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) реализация вызван `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="f7b37-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="f7b37-187">Этот класс можно найти в **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** проекта.</span><span class="sxs-lookup"><span data-stu-id="f7b37-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="f7b37-188">При создании экземпляра `TextBlockWriter`, передайте в текущем [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)и [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) где он создаст [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) для трассировки выходные данные:</span><span class="sxs-lookup"><span data-stu-id="f7b37-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="f7b37-189">Затем записать выходные данные трассировки в новый [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) в [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) передается в:</span><span class="sxs-lookup"><span data-stu-id="f7b37-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="f7b37-190">Ведение журнала событий клиента Windows Phone в консоль отладки</span><span class="sxs-lookup"><span data-stu-id="f7b37-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="f7b37-191">Для отправки выходных данных консоли отладки, а не пользовательского интерфейса, создать реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , записывает в окно отладки и назначьте его для подключения к [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) свойство:</span><span class="sxs-lookup"><span data-stu-id="f7b37-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="f7b37-192">Затем данные трассировки записываются в окно отладки в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f7b37-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="f7b37-193">Включение трассировки в клиенте JavaScript</span><span class="sxs-lookup"><span data-stu-id="f7b37-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="f7b37-194">Чтобы включить ведение журнала на стороне клиента для подключения, задайте `logging` свойство в объекте подключения, перед вызовом метода `start` метод, чтобы установить соединение.</span><span class="sxs-lookup"><span data-stu-id="f7b37-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="f7b37-195">**Клиентский код JavaScript для включения трассировки на консоль обозревателя (с помощью созданного прокси-сервер)**</span><span class="sxs-lookup"><span data-stu-id="f7b37-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="f7b37-196">**Клиентский код JavaScript для включения трассировки на консоль обозревателя (без созданный прокси-сервера)**</span><span class="sxs-lookup"><span data-stu-id="f7b37-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="f7b37-197">Если трассировка включена, клиент JavaScript записывает события в консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="f7b37-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="f7b37-198">Чтобы получить доступ к консоли браузера, см. в разделе [мониторинга транспортов](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="f7b37-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="f7b37-199">На следующем рисунке показан клиент SignalR JavaScript с включенной трассировкой.</span><span class="sxs-lookup"><span data-stu-id="f7b37-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="f7b37-200">В консоли браузера показано подключение и вызов событий центра:</span><span class="sxs-lookup"><span data-stu-id="f7b37-200">It shows connection and hub invocation events in the browser console:</span></span>

![События трассировки SignalR в консоли браузера](enabling-signalr-tracing/_static/image4.png)
