---
uid: signalr/overview/performance/signalr-performance
title: Производительность SignalR | Документация Майкрософт
author: bradygaster
description: Производительность SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 3326c2e600854fc7a4435d96c45b04a6188d3937
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836516"
---
<a name="signalr-performance"></a><span data-ttu-id="f75b2-103">Производительность SignalR</span><span class="sxs-lookup"><span data-stu-id="f75b2-103">SignalR Performance</span></span>
====================
<span data-ttu-id="f75b2-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f75b2-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="f75b2-105">В этом разделе описывается, как спроектировать архитектуру, измерения и повышения производительности в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f75b2-106">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="f75b2-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="f75b2-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f75b2-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f75b2-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f75b2-108">.NET 4.5</span></span>
> - <span data-ttu-id="f75b2-109">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="f75b2-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f75b2-110">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="f75b2-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="f75b2-111">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f75b2-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f75b2-112">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="f75b2-112">Questions and comments</span></span>
>
> <span data-ttu-id="f75b2-113">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="f75b2-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f75b2-114">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f75b2-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f75b2-115">Недавней презентации на производительность SignalR и масштабирования, см. в разделе [масштабирование в Интернете в реальном времени с помощью ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="f75b2-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="f75b2-116">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="f75b2-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f75b2-117">Рекомендации по проектированию</span><span class="sxs-lookup"><span data-stu-id="f75b2-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="f75b2-118">Помощник по настройке производительность сервера SignalR</span><span class="sxs-lookup"><span data-stu-id="f75b2-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="f75b2-119">Устранение неполадок производительности</span><span class="sxs-lookup"><span data-stu-id="f75b2-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="f75b2-120">Использование счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="f75b2-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="f75b2-121">Использование других счетчиков производительности</span><span class="sxs-lookup"><span data-stu-id="f75b2-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="f75b2-122">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="f75b2-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="f75b2-123">Рекомендации по проектированию</span><span class="sxs-lookup"><span data-stu-id="f75b2-123">Design considerations</span></span>

<span data-ttu-id="f75b2-124">В этом разделе описаны шаблоны, которые можно реализовать при разработке приложения SignalR, чтобы убедиться, что не выполняется ухудшить производительность путем создания лишний сетевой трафик.</span><span class="sxs-lookup"><span data-stu-id="f75b2-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="f75b2-125">Частота запросов сообщений</span><span class="sxs-lookup"><span data-stu-id="f75b2-125">Throttling message frequency</span></span>

<span data-ttu-id="f75b2-126">Даже в приложении, отправки сообщений с высокой частотой (например, приложение игры в реальном времени) большинство приложений не нужно отправлять несколько сообщений в секунду.</span><span class="sxs-lookup"><span data-stu-id="f75b2-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="f75b2-127">Чтобы уменьшить объем трафика, создаваемую каждый клиент, цикл обработки сообщений можно реализовать, очереди и отправки сообщений не более часто фиксированной ставке (то есть до определенного числа сообщений будут отправляться каждую секунду, при наличии сообщений в это время в Интервал отправки).</span><span class="sxs-lookup"><span data-stu-id="f75b2-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="f75b2-128">Пример приложения, который регулирует поток сообщений, определенных скорость (от сервера и клиента), см. в разделе [высокой частотой в реальном времени с SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f75b2-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="f75b2-129">Уменьшая размер сообщения</span><span class="sxs-lookup"><span data-stu-id="f75b2-129">Reducing message size</span></span>

<span data-ttu-id="f75b2-130">Можно уменьшить размер сообщения SignalR, уменьшив размер сериализованных объектов.</span><span class="sxs-lookup"><span data-stu-id="f75b2-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="f75b2-131">В серверном коде, если вы отправляете объект, содержащий свойства, которые не должны передаваться, предотвратить эти свойства сериализацию с помощью `JsonIgnore` атрибута.</span><span class="sxs-lookup"><span data-stu-id="f75b2-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="f75b2-132">Имена свойств, также сохраняются в сообщение; имена свойств может быть сокращено с помощью `JsonProperty` атрибута.</span><span class="sxs-lookup"><span data-stu-id="f75b2-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="f75b2-133">В следующем образце кода показано, как исключить свойство отправку клиенту и сокращение имена свойств:</span><span class="sxs-lookup"><span data-stu-id="f75b2-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="f75b2-134">**Код сервера .NET, который демонстрирует атрибут JsonIgnore, чтобы исключить данные отправку клиенту и атрибут JsonProperty, чтобы уменьшить размер сообщения**</span><span class="sxs-lookup"><span data-stu-id="f75b2-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="f75b2-135">Чтобы сохранить удобочитаемость / сопровождения в клиентском коде имена сокращенное свойство может быть пересопоставленный в понятном имена после получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="f75b2-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="f75b2-136">В следующем образце кода демонстрируется один из возможных способов повторного сопоставления названия для больше из них, путем определения контракта сообщения (сопоставление) и с помощью `reMap` функция, применяемая к классу оптимизированного сообщений контракта:</span><span class="sxs-lookup"><span data-stu-id="f75b2-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="f75b2-137">**Код JavaScript на стороне клиента, который указывает, что используется сокращение имен свойств понятное именами**</span><span class="sxs-lookup"><span data-stu-id="f75b2-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="f75b2-138">Имена может быть сокращено в сообщениях от клиента к серверу, используя тот же метод.</span><span class="sxs-lookup"><span data-stu-id="f75b2-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="f75b2-139">Используемый объем памяти (то есть, объем памяти, используемой для сообщения) сообщения объект также может повысить производительность.</span><span class="sxs-lookup"><span data-stu-id="f75b2-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="f75b2-140">Например если полный спектр `int` не требуется, `short` или `byte` взамен можно использовать.</span><span class="sxs-lookup"><span data-stu-id="f75b2-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="f75b2-141">Так как сообщения хранятся в канале сообщений в памяти сервера, уменьшение размера сообщения также могут устранить проблемы памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="f75b2-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="f75b2-142">Помощник по настройке производительность сервера SignalR</span><span class="sxs-lookup"><span data-stu-id="f75b2-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="f75b2-143">Следующие параметры конфигурации можно использовать для настройки сервера для повышения производительности в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="f75b2-144">Общие сведения о том, как повысить производительность в приложении ASP.NET, см. в разделе [повышение производительности ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="f75b2-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="f75b2-145">**Параметры конфигурации SignalR**</span><span class="sxs-lookup"><span data-stu-id="f75b2-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="f75b2-146">**DefaultMessageBufferSize**: По умолчанию SignalR сохраняет 1000 сообщений в памяти на каждый центр на соединение.</span><span class="sxs-lookup"><span data-stu-id="f75b2-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="f75b2-147">Если используются большие сообщения, это может создать проблемы с памятью, которые можно устранить, уменьшая это значение.</span><span class="sxs-lookup"><span data-stu-id="f75b2-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="f75b2-148">Этот параметр можно задать в `Application_Start` обработчик событий в приложении ASP.NET или в `Configuration` метод класса запуска OWIN в резидентного приложения.</span><span class="sxs-lookup"><span data-stu-id="f75b2-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="f75b2-149">В следующем образце показано, как уменьшить это значение, чтобы сократить использование памяти приложения, чтобы сократить объем используемой памяти сервера:</span><span class="sxs-lookup"><span data-stu-id="f75b2-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="f75b2-150">**.NET серверного кода в файле Startup.cs уменьшение размера буфера сообщения по умолчанию**</span><span class="sxs-lookup"><span data-stu-id="f75b2-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="f75b2-151">**Параметры конфигурации IIS**</span><span class="sxs-lookup"><span data-stu-id="f75b2-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="f75b2-152">**Максимум одновременных запросов в приложение**: Увеличение количества одновременных IIS запросов будет увеличить ресурсы сервера, доступных для обслуживания запросов.</span><span class="sxs-lookup"><span data-stu-id="f75b2-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="f75b2-153">Значение по умолчанию — 5000; Чтобы увеличить этот параметр, выполните следующие команды в командной строке с повышенными правами:</span><span class="sxs-lookup"><span data-stu-id="f75b2-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="f75b2-154">**ApplicationPool QueueLength**: Это максимальное число запросов, что Http.sys помещает в очередь для пула приложений.</span><span class="sxs-lookup"><span data-stu-id="f75b2-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="f75b2-155">Когда очередь заполнена, новые запросы ответ 503 «Служба недоступна».</span><span class="sxs-lookup"><span data-stu-id="f75b2-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="f75b2-156">Значение по умолчанию — 1000.</span><span class="sxs-lookup"><span data-stu-id="f75b2-156">The default value is 1000.</span></span>

    <span data-ttu-id="f75b2-157">Сокращение длины очереди для рабочего процесса в пуле приложений, размещение приложения будет экономии ресурсов памяти.</span><span class="sxs-lookup"><span data-stu-id="f75b2-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="f75b2-158">Дополнительные сведения см. в разделе [управление, помощник по настройке и Настройка групп приложений](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="f75b2-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="f75b2-159">**Параметры конфигурации ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f75b2-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="f75b2-160">Этот раздел содержит параметры конфигурации, которые могут устанавливаться в `aspnet.config` файл.</span><span class="sxs-lookup"><span data-stu-id="f75b2-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="f75b2-161">Этот файл находится в одном из двух расположений, в зависимости от платформы:</span><span class="sxs-lookup"><span data-stu-id="f75b2-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="f75b2-162">Следующие параметры ASP.NET, которые могут повысить производительность SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="f75b2-163">**Максимальное число параллельных запросов на один ЦП**: Увеличение значения этого параметра может устранить узкие места производительности.</span><span class="sxs-lookup"><span data-stu-id="f75b2-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="f75b2-164">Чтобы увеличить этот параметр, добавьте следующий параметр конфигурации в `aspnet.config` файла:</span><span class="sxs-lookup"><span data-stu-id="f75b2-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="f75b2-165">**Предел очереди запросов**: Если общее число подключений превышает `maxConcurrentRequestsPerCPU` параметр ASP.NET начнется регулирование запросов с использованием очереди.</span><span class="sxs-lookup"><span data-stu-id="f75b2-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="f75b2-166">Чтобы увеличить размер очереди, можно увеличить `requestQueueLimit` параметр.</span><span class="sxs-lookup"><span data-stu-id="f75b2-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="f75b2-167">Чтобы сделать это, добавьте следующий параметр конфигурации в `processModel` узел в `config/machine.config` (а не `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="f75b2-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="f75b2-168">Устранение неполадок производительности</span><span class="sxs-lookup"><span data-stu-id="f75b2-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="f75b2-169">В этом разделе описываются способы поиска узких мест производительности в приложении.</span><span class="sxs-lookup"><span data-stu-id="f75b2-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="f75b2-170">Проверка того, что используется WebSocket</span><span class="sxs-lookup"><span data-stu-id="f75b2-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="f75b2-171">Хотя SignalR можно использовать различные транспорты для обмена данными между клиентом и сервером, WebSocket предлагает это значительно быстрее и следует использовать, если клиент и сервер поддерживают его.</span><span class="sxs-lookup"><span data-stu-id="f75b2-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="f75b2-172">Чтобы определить, если клиент и сервер соответствия требованиям к WebSocket, см. в разделе [транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f75b2-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="f75b2-173">Чтобы определить, какой транспорт используется в приложении, можно использовать средства разработчика браузера и изучить журналы, чтобы узнать, какой транспорт используется для подключения.</span><span class="sxs-lookup"><span data-stu-id="f75b2-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="f75b2-174">Сведения об использовании средства разработки браузера в Internet Explorer и Chrome, см. в разделе [транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f75b2-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="f75b2-175">Использование счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="f75b2-175">Using SignalR performance counters</span></span>

<span data-ttu-id="f75b2-176">В этом разделе описывается включение и использование счетчиков производительности SignalR, найден в `Microsoft.AspNet.SignalR.Utils` пакета.</span><span class="sxs-lookup"><span data-stu-id="f75b2-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="f75b2-177">Установка signalr.exe</span><span class="sxs-lookup"><span data-stu-id="f75b2-177">Installing signalr.exe</span></span>

<span data-ttu-id="f75b2-178">Счетчики производительности можно добавить на сервер, с помощью служебной программы, называется SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="f75b2-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="f75b2-179">Чтобы установить эту программу, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f75b2-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="f75b2-180">В Visual Studio выберите **средства** > **диспетчер пакетов NuGet** > **управление пакетами NuGet для решения**</span><span class="sxs-lookup"><span data-stu-id="f75b2-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="f75b2-181">Поиск **signalr.utils**и выберите команду установить.</span><span class="sxs-lookup"><span data-stu-id="f75b2-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="f75b2-182">Примите лицензионное соглашение для установки пакета.</span><span class="sxs-lookup"><span data-stu-id="f75b2-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="f75b2-183">Будет установлен SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="f75b2-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="f75b2-184">Установка счетчиков производительности с SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="f75b2-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="f75b2-185">Чтобы установить счетчиков производительности SignalR, запустите SignalR.exe в командную строку со следующим параметром:</span><span class="sxs-lookup"><span data-stu-id="f75b2-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="f75b2-186">Чтобы удалить счетчики производительности SignalR, выполните SignalR.exe в командную строку со следующим параметром:</span><span class="sxs-lookup"><span data-stu-id="f75b2-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="f75b2-187">Счетчики производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="f75b2-187">SignalR Performance counters</span></span>

<span data-ttu-id="f75b2-188">Служебные программы пакет устанавливает следующие счетчики производительности.</span><span class="sxs-lookup"><span data-stu-id="f75b2-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="f75b2-189">Счетчики «Всего» измеряет число событий с момента последнего пул приложений или сервере перезапуска.</span><span class="sxs-lookup"><span data-stu-id="f75b2-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="f75b2-190">**Метрик подключения**</span><span class="sxs-lookup"><span data-stu-id="f75b2-190">**Connection metrics**</span></span>

<span data-ttu-id="f75b2-191">Следующие метрики измерения возникающих событий время существования подключения.</span><span class="sxs-lookup"><span data-stu-id="f75b2-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="f75b2-192">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f75b2-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="f75b2-193">**Подключений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-193">**Connections Connected**</span></span>
- <span data-ttu-id="f75b2-194">**Подключения повторно присоединена**</span><span class="sxs-lookup"><span data-stu-id="f75b2-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="f75b2-195">**Подключения к отключении**</span><span class="sxs-lookup"><span data-stu-id="f75b2-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="f75b2-196">**Текущих подключений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-196">**Connections Current**</span></span>

<span data-ttu-id="f75b2-197">**Метрики использования**</span><span class="sxs-lookup"><span data-stu-id="f75b2-197">**Message metrics**</span></span>

<span data-ttu-id="f75b2-198">Следующие метрики измерения сообщение трафик, создаваемый SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="f75b2-199">**Всего получено сообщение соединения**</span><span class="sxs-lookup"><span data-stu-id="f75b2-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="f75b2-200">**Всего отправлено сообщение соединения**</span><span class="sxs-lookup"><span data-stu-id="f75b2-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="f75b2-201">**Подключение сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="f75b2-202">**Подключения сообщений, отправляемых в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="f75b2-203">**Метрики шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-203">**Message bus metrics**</span></span>

<span data-ttu-id="f75b2-204">Следующие метрики измерения трафика через внутренний сообщений SignalR, очереди, в которой размещаются всех входящих и исходящих сообщений SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="f75b2-205">Сообщение **опубликовано** при его отправке или вещания.</span><span class="sxs-lookup"><span data-stu-id="f75b2-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="f75b2-206">Объект **подписчика** в данном контексте — это подписка на канал сообщений; это должно быть равно количество клиентов, а также сам сервер.</span><span class="sxs-lookup"><span data-stu-id="f75b2-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="f75b2-207">**Выделенной рабочей роли** — это компонент, который отправляет данные в активных соединений; **занятых рабочих** — это приложения, активно отправляет сообщение.</span><span class="sxs-lookup"><span data-stu-id="f75b2-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="f75b2-208">**Канал сообщений полученных всего сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="f75b2-209">**Канал сообщений сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f75b2-210">**Общее число опубликованных Message шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="f75b2-211">**Канал сообщений сообщения, опубликованные в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="f75b2-212">**Текущий подписчиков шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="f75b2-213">**Общее число подписчиков шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="f75b2-214">**Подписчики шины сообщений в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="f75b2-215">**Канал сообщений, выделенных рабочих ролей**</span><span class="sxs-lookup"><span data-stu-id="f75b2-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="f75b2-216">**Занятых рабочих процессов для шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="f75b2-217">**Текущий разделы шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="f75b2-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="f75b2-218">**Погрешность**</span><span class="sxs-lookup"><span data-stu-id="f75b2-218">**Error metrics**</span></span>

<span data-ttu-id="f75b2-219">Следующие метрики измерения ошибки, создаваемые трафиком сообщений SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="f75b2-220">**Разрешения центра** ошибки возникают, когда концентратор или метода концентратора, не может быть разрешена.</span><span class="sxs-lookup"><span data-stu-id="f75b2-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="f75b2-221">**Вызов концентратора** ошибки, исключения, возникшие во время вызова метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="f75b2-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="f75b2-222">**Транспорт** ошибки — ошибки подключения, создается во время HTTP-запроса или ответа.</span><span class="sxs-lookup"><span data-stu-id="f75b2-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="f75b2-223">**Ошибки: Общее число всех**</span><span class="sxs-lookup"><span data-stu-id="f75b2-223">**Errors: All Total**</span></span>
- <span data-ttu-id="f75b2-224">**Ошибки: Все в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="f75b2-225">**Ошибки: Общее разрешение концентратора**</span><span class="sxs-lookup"><span data-stu-id="f75b2-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="f75b2-226">**Ошибки: Разрешение концентратора в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="f75b2-227">**Ошибки: Общее число вызовов концентратора**</span><span class="sxs-lookup"><span data-stu-id="f75b2-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="f75b2-228">**Ошибки: Центр вызовов/сек**</span><span class="sxs-lookup"><span data-stu-id="f75b2-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="f75b2-229">**Ошибки: Всего транспорта**</span><span class="sxs-lookup"><span data-stu-id="f75b2-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="f75b2-230">**Ошибки: Транспорт за секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="f75b2-231">**Метрики горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="f75b2-231">**Scaleout metrics**</span></span>

<span data-ttu-id="f75b2-232">Следующие метрики измерения трафика и ошибки, создаваемые поставщиком горизонтального масштабирования.</span><span class="sxs-lookup"><span data-stu-id="f75b2-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="f75b2-233">Объект **Stream** в этом контексте в единицу масштабирования, используемый поставщиком горизонтального масштабирования; это таблицы, если используется SQL Server, раздел, если используется служебной шины и подписки, если используется Redis.</span><span class="sxs-lookup"><span data-stu-id="f75b2-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="f75b2-234">Каждый поток гарантирует упорядоченную операций чтения и записи; один поток является потенциальных узких мест масштабирования, поэтому можно увеличить количество потоков с целью сокращения появления узкого места.</span><span class="sxs-lookup"><span data-stu-id="f75b2-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="f75b2-235">Если используются несколько потоков, SignalR автоматически распространять сообщения (сегментов) для этих потоков способом, который гарантирует, что сообщения, отправленные из любого соединения расположены в порядке.</span><span class="sxs-lookup"><span data-stu-id="f75b2-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="f75b2-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) настройку перемещения по элементам, длина очереди отправки горизонтального масштабирования, обслуживается SignalR.</span><span class="sxs-lookup"><span data-stu-id="f75b2-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="f75b2-237">Установите в значение больше 0 будет помещать все сообщения в очереди отправки, который должны отправляться по одному настроенных объединительной платы сообщений.</span><span class="sxs-lookup"><span data-stu-id="f75b2-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="f75b2-238">Если размер очереди превышает настроенный длина, последующие вызовы для отправки будет завершаться сбоем с [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) пока число сообщений в очереди меньше, чем задано параметром еще раз.</span><span class="sxs-lookup"><span data-stu-id="f75b2-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="f75b2-239">Постановка в очередь по умолчанию отключена, так как реализованные соединительных панелях обычно имеют свои собственные очереди или управления потоком на месте.</span><span class="sxs-lookup"><span data-stu-id="f75b2-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="f75b2-240">В случае SQL Server организация пулов соединений эффективно ограничивает количество отправляет, происходящего в любой момент времени.</span><span class="sxs-lookup"><span data-stu-id="f75b2-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="f75b2-241">По умолчанию используется только один поток для SQL Server и Redis, пять потоков используются для служебной шины и их постановку в очередь отключена, но эти параметры можно изменить с помощью конфигурации на сервере SQL и служебной шины:</span><span class="sxs-lookup"><span data-stu-id="f75b2-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="f75b2-242">**Код сервера .NET для настройки таблицы count или длины очереди SQL Server объединительной платы**</span><span class="sxs-lookup"><span data-stu-id="f75b2-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="f75b2-243">**Код сервера .NET для настройки раздела count или длины очереди служебной шины объединительной платы**</span><span class="sxs-lookup"><span data-stu-id="f75b2-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="f75b2-244">Объект **буферизация** поток — это приложения, перешла в состояние faulted, когда поток находится в состоянии сбоя, все сообщения, отправляемые на объединительную плату завершится неудачей, немедленно в том случае, пока поток больше не произошла ошибка.</span><span class="sxs-lookup"><span data-stu-id="f75b2-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="f75b2-245">**Длина очереди отправки** — количество сообщений, которые были опубликованы, но еще не отправлены.</span><span class="sxs-lookup"><span data-stu-id="f75b2-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="f75b2-246">**Канал сообщений горизонтального масштабирования сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="f75b2-247">**Общее число потоков горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="f75b2-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="f75b2-248">**Горизонтального масштабирования выполняет потоковую передачу открытым**</span><span class="sxs-lookup"><span data-stu-id="f75b2-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="f75b2-249">**Буферизация потоков горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="f75b2-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="f75b2-250">**Всего ошибок горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="f75b2-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="f75b2-251">**Ошибок горизонтального масштабирования в секунду**</span><span class="sxs-lookup"><span data-stu-id="f75b2-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="f75b2-252">**Длину очереди отправленных сообщений горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="f75b2-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="f75b2-253">Дополнительные сведения об этих счетчиков, которые измерения, см. в разделе [масштабирование SignalR с помощью служебной шины Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="f75b2-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="f75b2-254">Использование других счетчиков производительности</span><span class="sxs-lookup"><span data-stu-id="f75b2-254">Using other performance counters</span></span>

<span data-ttu-id="f75b2-255">Следующие счетчики производительности также может быть полезными для наблюдения за производительностью приложения.</span><span class="sxs-lookup"><span data-stu-id="f75b2-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="f75b2-256">**Память**</span><span class="sxs-lookup"><span data-stu-id="f75b2-256">**Memory**</span></span>

- <span data-ttu-id="f75b2-257">Память .NET CLR\\байт во всех кучах (для w3wp)</span><span class="sxs-lookup"><span data-stu-id="f75b2-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="f75b2-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="f75b2-258">**ASP.NET**</span></span>

- <span data-ttu-id="f75b2-259">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="f75b2-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="f75b2-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="f75b2-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="f75b2-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="f75b2-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="f75b2-262">**ЦП**</span><span class="sxs-lookup"><span data-stu-id="f75b2-262">**CPU**</span></span>

- <span data-ttu-id="f75b2-263">Information\Processor загруженности процессора</span><span class="sxs-lookup"><span data-stu-id="f75b2-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="f75b2-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="f75b2-264">**TCP/IP**</span></span>

- <span data-ttu-id="f75b2-265">TCPv6/подключений</span><span class="sxs-lookup"><span data-stu-id="f75b2-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="f75b2-266">TCPv4/подключений</span><span class="sxs-lookup"><span data-stu-id="f75b2-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="f75b2-267">**Веб-службы**</span><span class="sxs-lookup"><span data-stu-id="f75b2-267">**Web Service**</span></span>

- <span data-ttu-id="f75b2-268">Веб-служба\текущих подключений</span><span class="sxs-lookup"><span data-stu-id="f75b2-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="f75b2-269">Веб-Service\Maximum подключений</span><span class="sxs-lookup"><span data-stu-id="f75b2-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="f75b2-270">**Работа с потоками**</span><span class="sxs-lookup"><span data-stu-id="f75b2-270">**Threading**</span></span>

- <span data-ttu-id="f75b2-271">.NET блокировки и потоки CLR\\текущих логических потоков</span><span class="sxs-lookup"><span data-stu-id="f75b2-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="f75b2-272">.NET блокировки и потоки CLR\\число текущих физических потоков</span><span class="sxs-lookup"><span data-stu-id="f75b2-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="f75b2-273">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="f75b2-273">Other Resources</span></span>

<span data-ttu-id="f75b2-274">Дополнительные сведения по мониторингу и настройке производительности ASP.NET см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="f75b2-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="f75b2-275">[Общие сведения о производительности ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="f75b2-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="f75b2-276">Использование потоков ASP.NET в IIS 7.5, IIS 7.0 и IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="f75b2-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="f75b2-277">&lt;пул приложений&gt; элемент (веб-параметры)</span><span class="sxs-lookup"><span data-stu-id="f75b2-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
