---
uid: signalr/overview/performance/signalr-performance
title: "SignalR производительности | Документы Microsoft"
author: pfletcher
description: "SignalR производительности"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance"></a><span data-ttu-id="39dee-103">SignalR производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-103">SignalR Performance</span></span>
====================
<span data-ttu-id="39dee-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="39dee-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="39dee-105">Описывается, как разрабатывать приложения для измерения и повысить производительность приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="39dee-106">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="39dee-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="39dee-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="39dee-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="39dee-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="39dee-108">.NET 4.5</span></span>
> - <span data-ttu-id="39dee-109">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="39dee-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="39dee-110">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="39dee-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="39dee-111">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="39dee-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="39dee-112">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="39dee-112">Questions and comments</span></span>
> 
> <span data-ttu-id="39dee-113">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="39dee-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="39dee-114">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="39dee-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="39dee-115">Недавней презентации на SignalR производительности и масштабируемости, в разделе [масштабирование в Интернете в режиме реального времени с помощью ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="39dee-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="39dee-116">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="39dee-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="39dee-117">Вопросы проектирования</span><span class="sxs-lookup"><span data-stu-id="39dee-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="39dee-118">Настройки сервера SignalR для повышения производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="39dee-119">Диагностика проблем производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="39dee-120">С помощью счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="39dee-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="39dee-121">Использование других счетчиков производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="39dee-122">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="39dee-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="39dee-123">Вопросы проектирования</span><span class="sxs-lookup"><span data-stu-id="39dee-123">Design considerations</span></span>

<span data-ttu-id="39dee-124">В этом разделе описываются шаблоны, которые могут быть применены во время разработки приложения SignalR, чтобы убедиться, что не выполняется ухудшить производительность путем создания ненужных сетевого трафика.</span><span class="sxs-lookup"><span data-stu-id="39dee-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="39dee-125">Частота запросов сообщений</span><span class="sxs-lookup"><span data-stu-id="39dee-125">Throttling message frequency</span></span>

<span data-ttu-id="39dee-126">Даже в приложении, которое отправляет сообщения с высокой частотой (например, приложения, игры в реальном времени) большинство приложений не требуется отправить несколько сообщений в секунду.</span><span class="sxs-lookup"><span data-stu-id="39dee-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="39dee-127">Чтобы уменьшить объем трафика, создаваемый каждым клиентом, цикл обработки сообщений может быть реализован, очередей и отправки сообщений не более часто фиксированной ставке (то есть до определенного числа сообщений будут отправляться каждую секунду, если сообщения в это время в terval отправки).</span><span class="sxs-lookup"><span data-stu-id="39dee-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="39dee-128">Образец приложения, регулирует сообщения для определенных коэффициент (от сервера и клиента), в разделе [высокой частотой в реальном времени с SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="39dee-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="39dee-129">Уменьшение размера сообщения</span><span class="sxs-lookup"><span data-stu-id="39dee-129">Reducing message size</span></span>

<span data-ttu-id="39dee-130">Можно уменьшить размер сообщения SignalR, уменьшив размер сериализованных объектов.</span><span class="sxs-lookup"><span data-stu-id="39dee-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="39dee-131">В серверном коде при отправке объект, содержащий свойства, которые не должны передаваться предотвратить эти свойства сериализацию с помощью `JsonIgnore` атрибута.</span><span class="sxs-lookup"><span data-stu-id="39dee-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="39dee-132">Имена свойств, также сохраняются в сообщении; имена свойств может быть сокращено с помощью `JsonProperty` атрибута.</span><span class="sxs-lookup"><span data-stu-id="39dee-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="39dee-133">В следующем образце кода показано, как исключить свойство отправку клиенту и сокращение имена свойств:</span><span class="sxs-lookup"><span data-stu-id="39dee-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="39dee-134">**Код сервера .NET, демонстрирующее атрибут JsonIgnore запрет данных, отправляемых клиенту, а атрибут JsonProperty, чтобы уменьшить размер сообщения**</span><span class="sxs-lookup"><span data-stu-id="39dee-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="39dee-135">Чтобы сохранить удобочитаемость и удобства поддержки в клиентском коде сокращенное свойство имена могут быть имена, преобразованное в понятную после получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="39dee-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="39dee-136">В следующем образце кода демонстрируется один из возможных способов переназначение сокращенные имена больше из них, путем определения контракта сообщения (сопоставление) и с помощью `reMap` функции для применения к класса оптимизированного сообщений контракта:</span><span class="sxs-lookup"><span data-stu-id="39dee-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="39dee-137">**Клиентский код JavaScript, который указывает, что сокращено имена свойств для восприятия имена**</span><span class="sxs-lookup"><span data-stu-id="39dee-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="39dee-138">Имена может быть сокращено в сообщениях от клиента к серверу, используя тот же метод.</span><span class="sxs-lookup"><span data-stu-id="39dee-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="39dee-139">Объем памяти (объем памяти, используемый для сообщения) сообщения объект также может повысить производительность.</span><span class="sxs-lookup"><span data-stu-id="39dee-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="39dee-140">Например если полный спектр `int` не является обязательным, `short` или `byte` можно использовать вместо него.</span><span class="sxs-lookup"><span data-stu-id="39dee-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="39dee-141">Поскольку сообщения хранятся в канале сообщений в памяти сервера, уменьшение размера сообщения также могут устранения неполадок памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="39dee-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="39dee-142">Настройки сервера SignalR для повышения производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="39dee-143">Можно использовать следующие параметры конфигурации для настройки сервера для повышения производительности в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="39dee-144">Общие сведения о том, как повысить производительность приложений ASP.NET см. в разделе [повышение производительности ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="39dee-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="39dee-145">**Параметры конфигурации SignalR**</span><span class="sxs-lookup"><span data-stu-id="39dee-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="39dee-146">**DefaultMessageBufferSize**: по умолчанию SignalR сохраняет 1000 сообщений в памяти на концентраторе на один сеанс.</span><span class="sxs-lookup"><span data-stu-id="39dee-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="39dee-147">Если используются большие сообщения, это может создать проблемы с памятью, которые могут быть облегчено уменьшая это значение.</span><span class="sxs-lookup"><span data-stu-id="39dee-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="39dee-148">Этот параметр можно задать в `Application_Start` обработчик событий в приложении ASP.NET или `Configuration` метод класса запуска OWIN в резидентного приложения.</span><span class="sxs-lookup"><span data-stu-id="39dee-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="39dee-149">В следующем образце показано, как уменьшить это значение, чтобы сократить использование памяти приложения, чтобы сократить объем используемой памяти сервера:</span><span class="sxs-lookup"><span data-stu-id="39dee-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="39dee-150">**.NET серверного кода в файле Startup.cs уменьшение размера буфера сообщений по умолчанию**</span><span class="sxs-lookup"><span data-stu-id="39dee-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="39dee-151">**Параметры конфигурации IIS**</span><span class="sxs-lookup"><span data-stu-id="39dee-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="39dee-152">**Максимальное количество параллельных запросов приложения**: увеличение числа одновременных IIS запросов будет увеличить ресурсы сервера, доступных для обслуживания запросов.</span><span class="sxs-lookup"><span data-stu-id="39dee-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="39dee-153">Значение по умолчанию — 5000; Чтобы увеличить этот параметр, выполните следующие команды в командную строку с повышенными привилегиями:</span><span class="sxs-lookup"><span data-stu-id="39dee-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="39dee-154">**Пул приложений QueueLength**: максимальное число запросов в очереди, Http.sys для пула приложений.</span><span class="sxs-lookup"><span data-stu-id="39dee-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="39dee-155">Если очередь заполнена, новые запросы возвращается сообщение 503 «Служба недоступна».</span><span class="sxs-lookup"><span data-stu-id="39dee-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="39dee-156">Значение по умолчанию — 1000.</span><span class="sxs-lookup"><span data-stu-id="39dee-156">The default value is 1000.</span></span>

    <span data-ttu-id="39dee-157">Сокращение длины очереди для рабочих процессов в пуле приложений, размещение приложения будет экономии ресурсов памяти.</span><span class="sxs-lookup"><span data-stu-id="39dee-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="39dee-158">Дополнительные сведения см. в разделе [управление, помощник по настройке и Настройка групп приложений](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="39dee-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="39dee-159">**Параметры конфигурации ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="39dee-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="39dee-160">Этот раздел содержит параметры конфигурации, которые могут быть установлены в `aspnet.config` файла.</span><span class="sxs-lookup"><span data-stu-id="39dee-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="39dee-161">Этот файл находится в одном из двух мест, в зависимости от платформы:</span><span class="sxs-lookup"><span data-stu-id="39dee-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="39dee-162">Следующие параметры ASP.NET, которые могут повысить производительность SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="39dee-163">**Максимальное число параллельных запросов на один ЦП**: увеличение значения этого параметра может устранить узкие места по производительности.</span><span class="sxs-lookup"><span data-stu-id="39dee-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="39dee-164">Чтобы увеличить этот параметр, добавьте следующий параметр конфигурации в `aspnet.config` файла:</span><span class="sxs-lookup"><span data-stu-id="39dee-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="39dee-165">**Предел очереди запросов**: Если превышает общее число подключений `maxConcurrentRequestsPerCPU` параметр ASP.NET запустится регулирование запросов с использованием очереди.</span><span class="sxs-lookup"><span data-stu-id="39dee-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="39dee-166">Чтобы увеличить размер очереди, можно увеличить `requestQueueLimit` параметр.</span><span class="sxs-lookup"><span data-stu-id="39dee-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="39dee-167">Чтобы сделать это, добавьте следующий параметр конфигурации в `processModel` узел в `config/machine.config` (а не `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="39dee-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="39dee-168">Диагностика проблем производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="39dee-169">Этот раздел описывает способы поиска узких мест производительности в приложении.</span><span class="sxs-lookup"><span data-stu-id="39dee-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="39dee-170">Проверка того, что используется WebSocket</span><span class="sxs-lookup"><span data-stu-id="39dee-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="39dee-171">SignalR можно использовать различные транспорты для обмена данными между клиентом и сервером, WebSocket предлагает значительное преимущество в производительности и следует использовать, если клиент и сервер поддерживают его.</span><span class="sxs-lookup"><span data-stu-id="39dee-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="39dee-172">Чтобы определить, если клиент и сервер соответствует требованиям для WebSocket, в разделе [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="39dee-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="39dee-173">Чтобы определить, какой транспорт используется в приложении, можно использовать инструменты разработчика браузера и изучите журналы, чтобы увидеть, какой транспорт используется для соединения.</span><span class="sxs-lookup"><span data-stu-id="39dee-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="39dee-174">Сведения об использовании средства разработки браузера в Internet Explorer и Chrome см. в разделе [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="39dee-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="39dee-175">С помощью счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="39dee-175">Using SignalR performance counters</span></span>

<span data-ttu-id="39dee-176">В этом разделе описывается включение и использование счетчиков производительности SignalR, найден в `Microsoft.AspNet.SignalR.Utils` пакета.</span><span class="sxs-lookup"><span data-stu-id="39dee-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="39dee-177">Установка signalr.exe</span><span class="sxs-lookup"><span data-stu-id="39dee-177">Installing signalr.exe</span></span>

<span data-ttu-id="39dee-178">Счетчики производительности можно добавить на сервер, с помощью служебной программы, называется SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="39dee-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="39dee-179">Чтобы установить эту программу, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="39dee-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="39dee-180">В приложении Visual Studio выберите **средства**, **диспетчер пакетов библиотеки**, **управление пакетами NuGet для решения...**</span><span class="sxs-lookup"><span data-stu-id="39dee-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="39dee-181">Поиск **signalr.utils**и выберите установку.</span><span class="sxs-lookup"><span data-stu-id="39dee-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="39dee-182">Примите лицензионное соглашение для установки пакета.</span><span class="sxs-lookup"><span data-stu-id="39dee-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="39dee-183">Будет установлен для SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="39dee-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="39dee-184">Установка счетчиков производительности с SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="39dee-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="39dee-185">Чтобы установить счетчиков производительности SignalR, запустите SignalR.exe в командную строку со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="39dee-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="39dee-186">Чтобы удалить счетчиков производительности SignalR, запустите SignalR.exe в командную строку со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="39dee-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="39dee-187">Счетчики производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="39dee-187">SignalR Performance counters</span></span>

<span data-ttu-id="39dee-188">Служебные программы пакет устанавливает следующие счетчики производительности.</span><span class="sxs-lookup"><span data-stu-id="39dee-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="39dee-189">Счетчики «Общее» измеряет число событий с момента последнего пул приложений или сервера перезапуска.</span><span class="sxs-lookup"><span data-stu-id="39dee-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="39dee-190">**Метрики подключения**</span><span class="sxs-lookup"><span data-stu-id="39dee-190">**Connection metrics**</span></span>

<span data-ttu-id="39dee-191">Следующие метрики измерения возникающих событий время существования подключения.</span><span class="sxs-lookup"><span data-stu-id="39dee-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="39dee-192">Дополнительные сведения см. в разделе [понимание и обработка события времени жизни соединений](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="39dee-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="39dee-193">**Подключений**</span><span class="sxs-lookup"><span data-stu-id="39dee-193">**Connections Connected**</span></span>
- <span data-ttu-id="39dee-194">**Переподключить подключений**</span><span class="sxs-lookup"><span data-stu-id="39dee-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="39dee-195">**Подключений отключен**</span><span class="sxs-lookup"><span data-stu-id="39dee-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="39dee-196">**Текущих подключений**</span><span class="sxs-lookup"><span data-stu-id="39dee-196">**Connections Current**</span></span>

<span data-ttu-id="39dee-197">**Метрики сообщения**</span><span class="sxs-lookup"><span data-stu-id="39dee-197">**Message metrics**</span></span>

<span data-ttu-id="39dee-198">Следующие метрики измерения трафика сообщения, сформированного SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="39dee-199">**Всего получено сообщений подключения**</span><span class="sxs-lookup"><span data-stu-id="39dee-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="39dee-200">**Всего отправлено сообщений подключения**</span><span class="sxs-lookup"><span data-stu-id="39dee-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="39dee-201">**Подключение сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="39dee-202">**Подключение отправленных сообщений в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="39dee-203">**Метрики шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="39dee-203">**Message bus metrics**</span></span>

<span data-ttu-id="39dee-204">Следующие метрики измерения трафика через внутренний сообщений SignalR, очереди, в которой размещаются всех входящих и исходящих сообщений SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="39dee-205">Сообщение **опубликовано** при его отправке или широковещательных пакетов.</span><span class="sxs-lookup"><span data-stu-id="39dee-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="39dee-206">Объект **подписчика** в этом контексте подписка на канал сообщений; это должно равняться количеству клиентов, а также сам сервер.</span><span class="sxs-lookup"><span data-stu-id="39dee-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="39dee-207">**Выделенных рабочих** — это компонент, который отправляет данные для активных подключений; **занятых рабочих** , активно отправляет сообщение.</span><span class="sxs-lookup"><span data-stu-id="39dee-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="39dee-208">**Канал сообщений сообщений в очереди полученных**</span><span class="sxs-lookup"><span data-stu-id="39dee-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="39dee-209">**Канал сообщений сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="39dee-210">**Всего опубликованных Message шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="39dee-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="39dee-211">**Канал сообщений сообщений, опубликованных в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="39dee-212">**Текущий подписчиков шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="39dee-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="39dee-213">**Всего подписчиков шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="39dee-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="39dee-214">**Подписчики шины сообщений в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="39dee-215">**Канал сообщений выделенных рабочих процессов**</span><span class="sxs-lookup"><span data-stu-id="39dee-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="39dee-216">**Сообщение шины рабочих процессов**</span><span class="sxs-lookup"><span data-stu-id="39dee-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="39dee-217">**Текущий разделы шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="39dee-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="39dee-218">**Ошибка метрик**</span><span class="sxs-lookup"><span data-stu-id="39dee-218">**Error metrics**</span></span>

<span data-ttu-id="39dee-219">Следующие метрики измерения ошибок, создаваемых трафиком сообщений SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="39dee-220">**Разрешение концентратора** ошибки возникают, когда не удается разрешить концентратора или метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="39dee-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="39dee-221">**Вызов концентратора** ошибки, исключения, возникшие во время вызова метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="39dee-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="39dee-222">**Транспорт** ошибки являются соединения ошибок, возникающих во время HTTP-запроса или ответа.</span><span class="sxs-lookup"><span data-stu-id="39dee-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="39dee-223">**Ошибок: Общее число всех**</span><span class="sxs-lookup"><span data-stu-id="39dee-223">**Errors: All Total**</span></span>
- <span data-ttu-id="39dee-224">**Ошибки: All/сек**</span><span class="sxs-lookup"><span data-stu-id="39dee-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="39dee-225">**Ошибок: Общее число концентратора разрешения**</span><span class="sxs-lookup"><span data-stu-id="39dee-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="39dee-226">**Ошибок: Разрешение концентратора в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="39dee-227">**Ошибок: Общее число вызовов концентратора**</span><span class="sxs-lookup"><span data-stu-id="39dee-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="39dee-228">**Ошибки: Концентратора вызовов/сек**</span><span class="sxs-lookup"><span data-stu-id="39dee-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="39dee-229">**Ошибок: Общее число транспорта**</span><span class="sxs-lookup"><span data-stu-id="39dee-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="39dee-230">**Ошибки: Транспорта в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="39dee-231">**Метрики горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="39dee-231">**Scaleout metrics**</span></span>

<span data-ttu-id="39dee-232">Следующие метрики измерения трафика и ошибки, созданные поставщиком горизонтального масштабирования.</span><span class="sxs-lookup"><span data-stu-id="39dee-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="39dee-233">Объект **поток** в этом контексте единица масштабирования, используемый поставщиком горизонтального масштабирования; это действие таблицы, если используется SQL Server, раздел, если используется Service Bus и подписки, если используется Redis.</span><span class="sxs-lookup"><span data-stu-id="39dee-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="39dee-234">Каждый поток гарантирует упорядоченную операций чтения и записи; один поток является потенциально узким местом шкалы, поэтому можно увеличить количество потоков для уменьшения, узким местом.</span><span class="sxs-lookup"><span data-stu-id="39dee-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="39dee-235">Если используются несколько потоков, SignalR автоматически распределяют сообщения (сегментов) эти потоки способом, что гарантирует, что сообщения, отправляемые из каждого соединения, в порядке.</span><span class="sxs-lookup"><span data-stu-id="39dee-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="39dee-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) настройки элементов управления длины очереди отправки горизонтального масштабирования, поддерживаемом SignalR.</span><span class="sxs-lookup"><span data-stu-id="39dee-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="39dee-237">Если его значение больше 0 будет помещать все сообщения в очереди отправки, отправляемых по одному настроенных объединительной платы сообщений.</span><span class="sxs-lookup"><span data-stu-id="39dee-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="39dee-238">Если размер очереди выходит за пределы настроенных длины, последующие вызовы для отправки будет завершаться немедленно [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) пока количество сообщений в очереди меньше, чем параметр еще раз.</span><span class="sxs-lookup"><span data-stu-id="39dee-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="39dee-239">Постановка в очередь по умолчанию отключена, так как реализованные соединительных панелях обычно имеют свои собственные очереди или управление потоком на месте.</span><span class="sxs-lookup"><span data-stu-id="39dee-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="39dee-240">В случае SQL Server эффективно пулов соединений ограничивает количество отправляет происходит в любой момент времени.</span><span class="sxs-lookup"><span data-stu-id="39dee-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="39dee-241">По умолчанию используется только один поток для SQL Server и Redis, пять потоков используются для Service Bus и постановка в очередь отключена, но эти параметры можно изменить с помощью конфигурации на SQL Server и Service Bus:</span><span class="sxs-lookup"><span data-stu-id="39dee-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="39dee-242">**Серверный код .NET для настройки длина count и очереди таблицы SQL Server объединительной платы**</span><span class="sxs-lookup"><span data-stu-id="39dee-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="39dee-243">**Серверный код .NET для настройки раздела длина count и очереди Service Bus объединительной платы**</span><span class="sxs-lookup"><span data-stu-id="39dee-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="39dee-244">Объект **буферизация** поток является тот, который вошел в состоянии сбоя, когда поток находится в состоянии сбоя, все сообщения, отправляемые на объединительную плату не будет работать сразу, пока поток больше не произошла ошибка.</span><span class="sxs-lookup"><span data-stu-id="39dee-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="39dee-245">**Длина очереди отправки** — количество сообщений, которые разнесены, но еще не отправлены.</span><span class="sxs-lookup"><span data-stu-id="39dee-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="39dee-246">**Канал сообщений горизонтального масштабирования сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="39dee-247">**Всего потоков горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="39dee-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="39dee-248">**Откройте потоки горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="39dee-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="39dee-249">**Буферизация потоков горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="39dee-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="39dee-250">**Общее число ошибок горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="39dee-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="39dee-251">**Ошибок горизонтального масштабирования в секунду**</span><span class="sxs-lookup"><span data-stu-id="39dee-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="39dee-252">**Длина очереди отправки горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="39dee-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="39dee-253">Дополнительные сведения об измерении этих счетчиков см. в разделе [SignalR горизонтального масштабирования с Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="39dee-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="39dee-254">Использование других счетчиков производительности</span><span class="sxs-lookup"><span data-stu-id="39dee-254">Using other performance counters</span></span>

<span data-ttu-id="39dee-255">Следующие счетчики производительности также можно использовать для наблюдения за производительностью приложения.</span><span class="sxs-lookup"><span data-stu-id="39dee-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="39dee-256">**Memory**</span><span class="sxs-lookup"><span data-stu-id="39dee-256">**Memory**</span></span>

- <span data-ttu-id="39dee-257">Память CLR .NET\\байт во всех кучах (для w3wp)</span><span class="sxs-lookup"><span data-stu-id="39dee-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="39dee-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="39dee-258">**ASP.NET**</span></span>

- <span data-ttu-id="39dee-259">ASP.net\текущих запросов</span><span class="sxs-lookup"><span data-stu-id="39dee-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="39dee-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="39dee-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="39dee-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="39dee-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="39dee-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="39dee-262">**CPU**</span></span>

- <span data-ttu-id="39dee-263">Information\Processor загруженности процессора</span><span class="sxs-lookup"><span data-stu-id="39dee-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="39dee-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="39dee-264">**TCP/IP**</span></span>

- <span data-ttu-id="39dee-265">TCP версии 6 и подключений</span><span class="sxs-lookup"><span data-stu-id="39dee-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="39dee-266">TCP версии 4 подключений</span><span class="sxs-lookup"><span data-stu-id="39dee-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="39dee-267">**Веб-службы**</span><span class="sxs-lookup"><span data-stu-id="39dee-267">**Web Service**</span></span>

- <span data-ttu-id="39dee-268">Веб-служба\текущих подключений</span><span class="sxs-lookup"><span data-stu-id="39dee-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="39dee-269">Веб-Service\Maximum подключений</span><span class="sxs-lookup"><span data-stu-id="39dee-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="39dee-270">**Работа с потоками**</span><span class="sxs-lookup"><span data-stu-id="39dee-270">**Threading**</span></span>

- <span data-ttu-id="39dee-271">.NET блокировки и потоки CLR\\текущих логических потоков</span><span class="sxs-lookup"><span data-stu-id="39dee-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="39dee-272">.NET блокировки и потоки CLR\\текущих физических потоков</span><span class="sxs-lookup"><span data-stu-id="39dee-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="39dee-273">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="39dee-273">Other Resources</span></span>

<span data-ttu-id="39dee-274">Дополнительные сведения о производительности ASP.NET, контроля и настройки см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="39dee-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="39dee-275">[Общие сведения о производительности ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="39dee-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="39dee-276">Использование потоков ASP.NET в IIS 7.5, IIS 7.0 и IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="39dee-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="39dee-277">&lt;пул приложений&gt; элемент (веб-параметров)</span><span class="sxs-lookup"><span data-stu-id="39dee-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
