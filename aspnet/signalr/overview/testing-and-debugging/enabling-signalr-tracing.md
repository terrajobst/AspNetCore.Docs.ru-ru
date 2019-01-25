---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Включение трассировки SignalR | Документация Майкрософт
author: bradygaster
description: В этом документе описывается, как включить и настроить трассировку для SignalR серверов и клиентов. Трассировка позволяет просмотреть диагностические сведения о событиях...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: c6515e3653798ef50e2d2dcb7354ffed407a190e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836471"
---
<a name="enabling-signalr-tracing"></a>Включение трассировки SignalR
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> В этом документе описывается, как включить и настроить трассировку для SignalR серверов и клиентов. Трассировка позволяет просмотреть диагностические сведения о событиях в приложении SignalR.
>
> В этом разделе был первоначально написан Майклом Патрик Флетчера.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4,5
> - SignalR версии 2
>
>
>
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
>
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


Если трассировка включена, приложение SignalR создает записи журнала для событий. Можно записывать события из клиента и сервера. Трассировка на соединение с сервером журналы, поставщике горизонтального масштабирования и события шины сообщений. Трассировка для событий подключения журналов клиента. В SignalR 2.1 и более поздних версиях трассировки на клиенте регистрирует полное содержимое сообщения вызова концентратора.

## <a name="contents"></a>Описание

- [Включение трассировки на сервере](#server)

    - [Ведение журнала событий сервера для текстовых файлов](#server_text)
    - [Ведение журнала событий сервера в журнал событий](#server_eventlog)
- [Включение трассировки клиента .NET (приложения для Windows Desktop)](#net_client)

    - [Ведение журнала событий клиента рабочего стола на консоль](#desktop_console)
    - [Ведение журнала событий клиента рабочего стола в текстовый файл](#desktop_text)
- [Включение трассировки в клиенты Windows Phone 8](#phone)

    - [Ведение журнала событий клиента Windows Phone в пользовательский интерфейс](#phone_ui)
    - [Ведение журнала событий клиента Windows Phone в консоль отладки](#phone_debug)
- [Включение трассировки в клиенте JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Включение трассировки на сервере

Трассировка выполняется на сервере в файле конфигурации приложения (App.config или Web.config в зависимости от типа проекта.) Можно указать, какие категории событий будут использоваться для входа. В файле конфигурации, вы также укажите, следует ли регистрировать события в текстовый файл, журнал событий Windows или пользовательский журнал, используя реализацию [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Категории событий сервера включают следующие типы сообщений:

| Исходный код | Сообщения |
| --- | --- |
| SignalR.SqlMessageBus | Установки поставщика SQL канала сообщений горизонтального масштабирования, операция базы данных, ошибки и события времени ожидания |
| SignalR.ServiceBusMessageBus | Создание раздела поставщика для горизонтального масштабирования службы шины и подписки, ошибок и событий сообщений |
| SignalR.RedisMessageBus | События подключения, отключения и ошибка поставщика горизонтального масштабирования redis |
| SignalR.ScaleoutMessageBus | Событий сообщений горизонтального масштабирования |
| SignalR.Transports.WebSocketTransport | События подключения, отключения, обмен сообщениями и ошибка транспорт WebSocket |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents транспорта подключение, отключение, обмен сообщениями и ошибка события |
| SignalR.Transports.ForeverFrameTransport | Транспортные ForeverFrame подключение, отключение, обмен сообщениями и ошибка события |
| SignalR.Transports.LongPollingTransport | Транспортные LongPolling подключение, отключение, обмен сообщениями и ошибка события |
| SignalR.Transports.TransportHeartBeat | События подключения, отключения и keepalive транспорта |
| SignalR.ReflectedHubDescriptorProvider | Концентратор событий обнаружения |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Ведение журнала событий сервера для текстовых файлов

Ниже показано, как включить трассировку для каждой категории событий. Этот пример настраивает приложение для регистрации событий в текстовый файл.

**XML-код сервера для включения трассировки**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

В приведенном выше коде в `SignalRSwitch` запись указывает [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) для событий, отправленных в указанном журнале. В этом случае оно имеет значение `Verbose` регистрируются означающее, что все сообщения отладки и трассировки.

В следующем примере вывод записей из `transports.log.txt` файла для приложения с помощью указанного выше файла конфигурации. Он иллюстрирует создание нового подключения, удаленные подключения и события пульса транспорта.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Ведение журнала событий сервера в журнал событий

Чтобы регистрировать события в журнале событий, а не в текстовый файл, измените значения записей в `sharedListeners` узла. Ниже показано, как регистрировать события сервера в журнале событий:

**XML-код сервера для регистрации событий в журнале событий**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

События регистрируются в журнале приложений и доступны через средство просмотра событий, как показано ниже:

![Средство просмотра событий, отображаться журналы SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> При использовании в журнал событий, установить **TraceLevel** для **ошибка** следует управляемое количество сообщений.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Включение трассировки клиента .NET (приложения для Windows Desktop)

Клиент .NET можно регистрировать события в консоль, текстовый файл, или пользовательский журнал, используя реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Чтобы включить ведение журнала в клиенте .NET, задайте соединение с `TraceLevel` свойства [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) значение и `TraceWriter` свойство на допустимый [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) экземпляра.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Ведение журнала событий клиента рабочего стола на консоль

Следующий код C# показано, как регистрировать события в клиенте .NET на консоль:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Ведение журнала событий клиента рабочего стола в текстовый файл

Следующий код C# показано, как регистрировать события в клиенте .NET в текстовый файл:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

В следующем примере вывод записей из `ClientLog.txt` файла для приложения с помощью указанного выше файла конфигурации. В нем показано клиент, подключающийся к серверу, и вызов клиентского метода концентратора вызывается `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Включение трассировки в клиенты Windows Phone 8

SignalR приложения для приложений Windows Phone используют один и тот же клиент .NET как классические приложения, но [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) и запись в файл с [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) недоступны. Вместо этого необходимо создать пользовательскую реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) для трассировки.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Ведение журнала событий клиента Windows Phone в пользовательский интерфейс

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) имеется пример Windows Phone, записывает выходные данные трассировки [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) с использованием пользовательской [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) реализация вызван `TextBlockWriter`. Этот класс можно найти в **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** проекта. При создании экземпляра `TextBlockWriter`, передайте в текущем [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)и [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) где он создаст [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) для трассировки выходные данные:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Затем записать выходные данные трассировки в новый [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) в [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) передается в:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Ведение журнала событий клиента Windows Phone в консоль отладки

Для отправки выходных данных консоли отладки, а не пользовательского интерфейса, создать реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , записывает в окно отладки и назначьте его для подключения к [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) свойство:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Затем данные трассировки записываются в окно отладки в Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Включение трассировки в клиенте JavaScript

Чтобы включить ведение журнала на стороне клиента для подключения, задайте `logging` свойство в объекте подключения, перед вызовом метода `start` метод, чтобы установить соединение.

**Клиентский код JavaScript для включения трассировки на консоль обозревателя (с помощью созданного прокси-сервер)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Клиентский код JavaScript для включения трассировки на консоль обозревателя (без созданный прокси-сервера)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Если трассировка включена, клиент JavaScript записывает события в консоли браузера. Чтобы получить доступ к консоли браузера, см. в разделе [мониторинга транспортов](../getting-started/introduction-to-signalr.md#MonitoringTransports).

На следующем рисунке показан клиент SignalR JavaScript с включенной трассировкой. В консоли браузера показано подключение и вызов событий центра:

![События трассировки SignalR в консоли браузера](enabling-signalr-tracing/_static/image4.png)
