---
uid: signalr/overview/older-versions/signalr-performance
title: Производительность SignalR (SignalR 1.x) | Документация Майкрософт
author: pfletcher
description: Производительность SignalR
ms.author: riande
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: ea2d3908544ac8b3ea17ceceaf1d2905c5c6f322
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287585"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="c5c03-103">Производительность SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="c5c03-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="c5c03-104">по [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c5c03-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c5c03-105">В этом разделе описывается, как спроектировать архитектуру, измерения и повышения производительности в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5c03-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="c5c03-106">Недавней презентации на производительность SignalR и масштабирования, см. в разделе [масштабирование в Интернете в реальном времени с помощью ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="c5c03-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="c5c03-107">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="c5c03-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="c5c03-108">Рекомендации по проектированию</span><span class="sxs-lookup"><span data-stu-id="c5c03-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="c5c03-109">Помощник по настройке производительность сервера SignalR</span><span class="sxs-lookup"><span data-stu-id="c5c03-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="c5c03-110">Устранение неполадок производительности</span><span class="sxs-lookup"><span data-stu-id="c5c03-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="c5c03-111">Использование счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="c5c03-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="c5c03-112">Использование других счетчиков производительности</span><span class="sxs-lookup"><span data-stu-id="c5c03-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="c5c03-113">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="c5c03-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="c5c03-114">Рекомендации по проектированию</span><span class="sxs-lookup"><span data-stu-id="c5c03-114">Design considerations</span></span>

<span data-ttu-id="c5c03-115">В этом разделе описаны шаблоны, которые можно реализовать при разработке приложения SignalR, чтобы убедиться, что не выполняется ухудшить производительность путем создания лишний сетевой трафик.</span><span class="sxs-lookup"><span data-stu-id="c5c03-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="c5c03-116">Частота запросов сообщений</span><span class="sxs-lookup"><span data-stu-id="c5c03-116">Throttling message frequency</span></span>

<span data-ttu-id="c5c03-117">Даже в приложении, отправки сообщений с высокой частотой (например, приложение игры в реальном времени) большинство приложений не нужно отправлять несколько сообщений в секунду.</span><span class="sxs-lookup"><span data-stu-id="c5c03-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="c5c03-118">Чтобы уменьшить объем трафика, создаваемую каждый клиент, цикл обработки сообщений можно реализовать, очереди и отправки сообщений не более часто фиксированной ставке (то есть до определенного числа сообщений будут отправляться каждую секунду, при наличии сообщений в это время в Интервал отправки).</span><span class="sxs-lookup"><span data-stu-id="c5c03-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="c5c03-119">Пример приложения, который регулирует поток сообщений, определенных скорость (от сервера и клиента), см. в разделе [высокой частотой в реальном времени с SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c5c03-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="c5c03-120">Уменьшая размер сообщения</span><span class="sxs-lookup"><span data-stu-id="c5c03-120">Reducing message size</span></span>

<span data-ttu-id="c5c03-121">Можно уменьшить размер сообщения SignalR, уменьшив размер сериализованных объектов.</span><span class="sxs-lookup"><span data-stu-id="c5c03-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="c5c03-122">В серверном коде, если вы отправляете объект, содержащий свойства, которые не должны передаваться, предотвратить эти свойства сериализацию с помощью `JsonIgnore` атрибута.</span><span class="sxs-lookup"><span data-stu-id="c5c03-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="c5c03-123">Имена свойств, также сохраняются в сообщение; имена свойств может быть сокращено с помощью `JsonProperty` атрибута.</span><span class="sxs-lookup"><span data-stu-id="c5c03-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="c5c03-124">В следующем образце кода показано, как исключить свойство отправку клиенту и сокращение имена свойств:</span><span class="sxs-lookup"><span data-stu-id="c5c03-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="c5c03-125">**Код сервера .NET, который демонстрирует атрибут JsonIgnore, чтобы исключить данные отправку клиенту и атрибут JsonProperty, чтобы уменьшить размер сообщения**</span><span class="sxs-lookup"><span data-stu-id="c5c03-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="c5c03-126">Чтобы сохранить удобочитаемость / сопровождения в клиентском коде имена сокращенное свойство может быть пересопоставленный в понятном имена после получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="c5c03-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="c5c03-127">В следующем образце кода демонстрируется один из возможных способов повторного сопоставления названия для больше из них, путем определения контракта сообщения (сопоставление) и с помощью `reMap` функция, применяемая к классу оптимизированного сообщений контракта:</span><span class="sxs-lookup"><span data-stu-id="c5c03-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="c5c03-128">**Код JavaScript на стороне клиента, который указывает, что используется сокращение имен свойств понятное именами**</span><span class="sxs-lookup"><span data-stu-id="c5c03-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="c5c03-129">Имена может быть сокращено в сообщениях от клиента к серверу, используя тот же метод.</span><span class="sxs-lookup"><span data-stu-id="c5c03-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="c5c03-130">Используемый объем памяти (то есть, объем памяти, используемой для сообщения) сообщения объект также может повысить производительность.</span><span class="sxs-lookup"><span data-stu-id="c5c03-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="c5c03-131">Например если полный спектр `int` не требуется, `short` или `byte` взамен можно использовать.</span><span class="sxs-lookup"><span data-stu-id="c5c03-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="c5c03-132">Так как сообщения хранятся в канале сообщений в памяти сервера, уменьшение размера сообщения также могут устранить проблемы памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="c5c03-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="c5c03-133">Помощник по настройке производительность сервера SignalR</span><span class="sxs-lookup"><span data-stu-id="c5c03-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="c5c03-134">Следующие параметры конфигурации можно использовать для настройки сервера для повышения производительности в приложении SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5c03-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="c5c03-135">Общие сведения о том, как повысить производительность в приложении ASP.NET, см. в разделе [повышение производительности ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5c03-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="c5c03-136">**Параметры конфигурации SignalR**</span><span class="sxs-lookup"><span data-stu-id="c5c03-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="c5c03-137">**DefaultMessageBufferSize**: По умолчанию SignalR сохраняет 1000 сообщений в памяти на каждый центр на соединение.</span><span class="sxs-lookup"><span data-stu-id="c5c03-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="c5c03-138">Если используются большие сообщения, это может создать проблемы с памятью, которые можно устранить, уменьшая это значение.</span><span class="sxs-lookup"><span data-stu-id="c5c03-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="c5c03-139">Этот параметр можно задать в `Application_Start` обработчик событий в приложении ASP.NET или в `Configuration` метод класса запуска OWIN в резидентного приложения.</span><span class="sxs-lookup"><span data-stu-id="c5c03-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="c5c03-140">В следующем образце показано, как уменьшить это значение, чтобы сократить использование памяти приложения, чтобы сократить объем используемой памяти сервера:</span><span class="sxs-lookup"><span data-stu-id="c5c03-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="c5c03-141">**Код сервера .NET в файле Global.asax для уменьшение размера буфера сообщения по умолчанию**</span><span class="sxs-lookup"><span data-stu-id="c5c03-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="c5c03-142">**Параметры конфигурации IIS**</span><span class="sxs-lookup"><span data-stu-id="c5c03-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="c5c03-143">**Максимум одновременных запросов в приложение**: Увеличение количества одновременных IIS запросов будет увеличить ресурсы сервера, доступных для обслуживания запросов.</span><span class="sxs-lookup"><span data-stu-id="c5c03-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="c5c03-144">Значение по умолчанию — 5000; Чтобы увеличить этот параметр, выполните следующие команды в командной строке с повышенными правами:</span><span class="sxs-lookup"><span data-stu-id="c5c03-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="c5c03-145">**Параметры конфигурации ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="c5c03-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="c5c03-146">Этот раздел содержит параметры конфигурации, которые могут устанавливаться в `aspnet.config` файл.</span><span class="sxs-lookup"><span data-stu-id="c5c03-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="c5c03-147">Этот файл находится в одном из двух расположений, в зависимости от платформы:</span><span class="sxs-lookup"><span data-stu-id="c5c03-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="c5c03-148">Следующие параметры ASP.NET, которые могут повысить производительность SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5c03-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="c5c03-149">**Максимальное число параллельных запросов на один ЦП**: Увеличение значения этого параметра может устранить узкие места производительности.</span><span class="sxs-lookup"><span data-stu-id="c5c03-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="c5c03-150">Чтобы увеличить этот параметр, добавьте следующий параметр конфигурации в `aspnet.config` файла:</span><span class="sxs-lookup"><span data-stu-id="c5c03-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="c5c03-151">**Предел очереди запросов**: Если общее число подключений превышает `maxConcurrentRequestsPerCPU` параметр ASP.NET начнется регулирование запросов с использованием очереди.</span><span class="sxs-lookup"><span data-stu-id="c5c03-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="c5c03-152">Чтобы увеличить размер очереди, можно увеличить `requestQueueLimit` параметр.</span><span class="sxs-lookup"><span data-stu-id="c5c03-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="c5c03-153">Чтобы сделать это, добавьте следующий параметр конфигурации в `processModel` узел в `config/machine.config` (а не `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="c5c03-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="c5c03-154">Устранение неполадок производительности</span><span class="sxs-lookup"><span data-stu-id="c5c03-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="c5c03-155">В этом разделе описываются способы поиска узких мест производительности в приложении.</span><span class="sxs-lookup"><span data-stu-id="c5c03-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="c5c03-156">Проверка того, что используется WebSocket</span><span class="sxs-lookup"><span data-stu-id="c5c03-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="c5c03-157">Хотя SignalR можно использовать различные транспорты для обмена данными между клиентом и сервером, WebSocket предлагает это значительно быстрее и следует использовать, если клиент и сервер поддерживают его.</span><span class="sxs-lookup"><span data-stu-id="c5c03-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="c5c03-158">Чтобы определить, если клиент и сервер соответствия требованиям к WebSocket, см. в разделе [транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="c5c03-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="c5c03-159">Чтобы определить, какой транспорт используется в приложении, можно использовать средства разработчика браузера и изучить журналы, чтобы узнать, какой транспорт используется для подключения.</span><span class="sxs-lookup"><span data-stu-id="c5c03-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="c5c03-160">Сведения об использовании средства разработки браузера в Internet Explorer и Chrome, см. в разделе [транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="c5c03-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="c5c03-161">Использование счетчиков производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="c5c03-161">Using SignalR performance counters</span></span>

<span data-ttu-id="c5c03-162">В этом разделе описывается включение и использование счетчиков производительности SignalR, найден в `Microsoft.AspNet.SignalR.Utils` пакета.</span><span class="sxs-lookup"><span data-stu-id="c5c03-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="c5c03-163">Установка signalr.exe</span><span class="sxs-lookup"><span data-stu-id="c5c03-163">Installing signalr.exe</span></span>

<span data-ttu-id="c5c03-164">Счетчики производительности можно добавить на сервер, с помощью служебной программы, называется SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="c5c03-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="c5c03-165">Чтобы установить эту программу, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c5c03-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="c5c03-166">В Visual Studio выберите **средства** > **диспетчер пакетов NuGet** > **управление пакетами NuGet для решения**</span><span class="sxs-lookup"><span data-stu-id="c5c03-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="c5c03-167">Поиск **signalr.utils**и выберите команду установить.</span><span class="sxs-lookup"><span data-stu-id="c5c03-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="c5c03-168">Примите лицензионное соглашение для установки пакета.</span><span class="sxs-lookup"><span data-stu-id="c5c03-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="c5c03-169">Будет установлен SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="c5c03-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="c5c03-170">Установка счетчиков производительности с SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="c5c03-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="c5c03-171">Чтобы установить счетчиков производительности SignalR, запустите SignalR.exe в командную строку со следующим параметром:</span><span class="sxs-lookup"><span data-stu-id="c5c03-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="c5c03-172">Чтобы удалить счетчики производительности SignalR, выполните SignalR.exe в командную строку со следующим параметром:</span><span class="sxs-lookup"><span data-stu-id="c5c03-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="c5c03-173">Счетчики производительности SignalR</span><span class="sxs-lookup"><span data-stu-id="c5c03-173">SignalR Performance counters</span></span>

<span data-ttu-id="c5c03-174">Служебные программы пакет устанавливает следующие счетчики производительности.</span><span class="sxs-lookup"><span data-stu-id="c5c03-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="c5c03-175">Счетчики «Всего» измеряет число событий с момента последнего пул приложений или сервере перезапуска.</span><span class="sxs-lookup"><span data-stu-id="c5c03-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="c5c03-176">**Метрик подключения**</span><span class="sxs-lookup"><span data-stu-id="c5c03-176">**Connection metrics**</span></span>

<span data-ttu-id="c5c03-177">Следующие метрики измерения возникающих событий время существования подключения.</span><span class="sxs-lookup"><span data-stu-id="c5c03-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="c5c03-178">Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="c5c03-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="c5c03-179">**Подключений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-179">**Connections Connected**</span></span>
- <span data-ttu-id="c5c03-180">**Подключения повторно присоединена**</span><span class="sxs-lookup"><span data-stu-id="c5c03-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="c5c03-181">**Disonnected подключений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="c5c03-182">**Текущих подключений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-182">**Connections Current**</span></span>

<span data-ttu-id="c5c03-183">**Метрики использования**</span><span class="sxs-lookup"><span data-stu-id="c5c03-183">**Message metrics**</span></span>

<span data-ttu-id="c5c03-184">Следующие метрики измерения сообщение трафик, создаваемый SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5c03-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="c5c03-185">**Всего получено сообщение соединения**</span><span class="sxs-lookup"><span data-stu-id="c5c03-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="c5c03-186">**Всего отправлено сообщение соединения**</span><span class="sxs-lookup"><span data-stu-id="c5c03-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="c5c03-187">**Подключение сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="c5c03-188">**Подключения сообщений, отправляемых в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="c5c03-189">**Метрики шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-189">**Message bus metrics**</span></span>

<span data-ttu-id="c5c03-190">Следующие метрики измерения трафика через внутренний сообщений SignalR, очереди, в которой размещаются всех входящих и исходящих сообщений SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5c03-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="c5c03-191">Сообщение **опубликовано** при его отправке или вещания.</span><span class="sxs-lookup"><span data-stu-id="c5c03-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="c5c03-192">Объект **подписчика** в данном контексте — это подписка на канал сообщений; это должно быть равно количество клиентов, а также сам сервер.</span><span class="sxs-lookup"><span data-stu-id="c5c03-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="c5c03-193">**Выделенной рабочей роли** — это компонент, который отправляет данные в активных соединений; **занятых рабочих** — это приложения, активно отправляет сообщение.</span><span class="sxs-lookup"><span data-stu-id="c5c03-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="c5c03-194">**Канал сообщений полученных всего сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="c5c03-195">**Канал сообщений сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="c5c03-196">**Общее число опубликованных Message шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="c5c03-197">**Канал сообщений сообщения, опубликованные в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="c5c03-198">**Текущий подписчиков шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="c5c03-199">**Общее число подписчиков шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="c5c03-200">**Подписчики шины сообщений в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="c5c03-201">**Канал сообщений, выделенных рабочих ролей**</span><span class="sxs-lookup"><span data-stu-id="c5c03-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="c5c03-202">**Занятых рабочих процессов для шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="c5c03-203">**Текущий разделы шины сообщений**</span><span class="sxs-lookup"><span data-stu-id="c5c03-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="c5c03-204">**Погрешность**</span><span class="sxs-lookup"><span data-stu-id="c5c03-204">**Error metrics**</span></span>

<span data-ttu-id="c5c03-205">Следующие метрики измерения ошибки, создаваемые трафиком сообщений SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5c03-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="c5c03-206">**Разрешения центра** ошибки возникают, когда концентратор или метода концентратора, не может быть разрешена.</span><span class="sxs-lookup"><span data-stu-id="c5c03-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="c5c03-207">**Вызов концентратора** ошибки, исключения, возникшие во время вызова метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c5c03-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="c5c03-208">**Транспорт** ошибки — ошибки подключения, создается во время HTTP-запроса или ответа.</span><span class="sxs-lookup"><span data-stu-id="c5c03-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="c5c03-209">**Ошибки: Общее число всех**</span><span class="sxs-lookup"><span data-stu-id="c5c03-209">**Errors: All Total**</span></span>
- <span data-ttu-id="c5c03-210">**Ошибки: Все в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="c5c03-211">**Ошибки: Общее разрешение концентратора**</span><span class="sxs-lookup"><span data-stu-id="c5c03-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="c5c03-212">**Ошибки: Разрешение концентратора в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="c5c03-213">**Ошибки: Общее число вызовов концентратора**</span><span class="sxs-lookup"><span data-stu-id="c5c03-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="c5c03-214">**Ошибки: Центр вызовов/сек**</span><span class="sxs-lookup"><span data-stu-id="c5c03-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="c5c03-215">**Ошибки: Всего транспорта**</span><span class="sxs-lookup"><span data-stu-id="c5c03-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="c5c03-216">**Ошибки: Транспорт за секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="c5c03-217">**Метрики горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="c5c03-217">**Scaleout metrics**</span></span>

<span data-ttu-id="c5c03-218">Следующие метрики измерения трафика и ошибки, создаваемые поставщиком горизонтального масштабирования.</span><span class="sxs-lookup"><span data-stu-id="c5c03-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="c5c03-219">Объект **Stream** в этом контексте в единицу масштабирования, используемый поставщиком горизонтального масштабирования; это таблицы, если используется SQL Server, раздел, если используется служебной шины и подписки, если используется Redis.</span><span class="sxs-lookup"><span data-stu-id="c5c03-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="c5c03-220">По умолчанию используется только один поток, но это значение можно увеличить путем его настройки на сервере SQL и служебной шины.</span><span class="sxs-lookup"><span data-stu-id="c5c03-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="c5c03-221">Объект **буферизация** поток — это приложения, перешла в состояние faulted, когда поток находится в состоянии сбоя, все сообщения, отправляемые на объединительную плату завершится неудачей, немедленно в том случае, пока поток больше не произошла ошибка.</span><span class="sxs-lookup"><span data-stu-id="c5c03-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="c5c03-222">**Длина очереди отправки** — количество сообщений, которые были опубликованы, но еще не отправлены.</span><span class="sxs-lookup"><span data-stu-id="c5c03-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="c5c03-223">**Канал сообщений горизонтального масштабирования сообщений, полученных в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="c5c03-224">**Общее число потоков горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="c5c03-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="c5c03-225">**Горизонтального масштабирования выполняет потоковую передачу открытым**</span><span class="sxs-lookup"><span data-stu-id="c5c03-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="c5c03-226">**Буферизация потоков горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="c5c03-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="c5c03-227">**Всего ошибок горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="c5c03-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="c5c03-228">**Ошибок горизонтального масштабирования в секунду**</span><span class="sxs-lookup"><span data-stu-id="c5c03-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="c5c03-229">**Длину очереди отправленных сообщений горизонтального масштабирования**</span><span class="sxs-lookup"><span data-stu-id="c5c03-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="c5c03-230">Дополнительные сведения об этих счетчиков, которые измерения, см. в разделе [масштабирование SignalR с помощью служебной шины Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="c5c03-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="c5c03-231">Использование других счетчиков производительности</span><span class="sxs-lookup"><span data-stu-id="c5c03-231">Using other performance counters</span></span>

<span data-ttu-id="c5c03-232">Следующие счетчики производительности также может быть полезными для наблюдения за производительностью приложения.</span><span class="sxs-lookup"><span data-stu-id="c5c03-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="c5c03-233">**Память**</span><span class="sxs-lookup"><span data-stu-id="c5c03-233">**Memory**</span></span>

- <span data-ttu-id="c5c03-234">Память .NET CLR # байт во всех кучах (для w3wp)</span><span class="sxs-lookup"><span data-stu-id="c5c03-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="c5c03-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="c5c03-235">**ASP.NET**</span></span>

- <span data-ttu-id="c5c03-236">ASP.net\текущих запросов</span><span class="sxs-lookup"><span data-stu-id="c5c03-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="c5c03-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="c5c03-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="c5c03-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="c5c03-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="c5c03-239">**ЦП**</span><span class="sxs-lookup"><span data-stu-id="c5c03-239">**CPU**</span></span>

- <span data-ttu-id="c5c03-240">Information\Processor загруженности процессора</span><span class="sxs-lookup"><span data-stu-id="c5c03-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="c5c03-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="c5c03-241">**TCP/IP**</span></span>

- <span data-ttu-id="c5c03-242">TCPv6/подключений</span><span class="sxs-lookup"><span data-stu-id="c5c03-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="c5c03-243">TCPv4/подключений</span><span class="sxs-lookup"><span data-stu-id="c5c03-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="c5c03-244">**Веб-службы**</span><span class="sxs-lookup"><span data-stu-id="c5c03-244">**Web Service**</span></span>

- <span data-ttu-id="c5c03-245">Веб-служба\текущих подключений</span><span class="sxs-lookup"><span data-stu-id="c5c03-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="c5c03-246">Веб-Service\Maximum подключений</span><span class="sxs-lookup"><span data-stu-id="c5c03-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="c5c03-247">**Работа с потоками**</span><span class="sxs-lookup"><span data-stu-id="c5c03-247">**Threading**</span></span>

- <span data-ttu-id="c5c03-248">.NET CLR LocksAndThreads\# текущих логических потоков</span><span class="sxs-lookup"><span data-stu-id="c5c03-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="c5c03-249">Потоки LocksAnd .NET CLR\# текущих физических потоков</span><span class="sxs-lookup"><span data-stu-id="c5c03-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="c5c03-250">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="c5c03-250">Other Resources</span></span>

<span data-ttu-id="c5c03-251">Дополнительные сведения по мониторингу и настройке производительности ASP.NET см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="c5c03-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="c5c03-252">[Общие сведения о производительности ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="c5c03-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="c5c03-253">Использование потоков ASP.NET в IIS 7.5, IIS 7.0 и IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="c5c03-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="c5c03-254">&lt;пул приложений&gt; элемент (веб-параметры)</span><span class="sxs-lookup"><span data-stu-id="c5c03-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
