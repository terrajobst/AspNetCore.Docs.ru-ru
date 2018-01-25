---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "Включение трассировки SignalR | Документы Microsoft"
author: tfitzmac
description: "Этот документ описывает, как включить и настроить трассировку для SignalR серверов и клиентов. Трассировка позволяет просматривать диагностические сведения о событиях..."
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
---
<a name="enabling-signalr-tracing"></a>Включение трассировки SignalR
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> Этот документ описывает, как включить и настроить трассировку для SignalR серверов и клиентов. Трассировка позволяет просматривать диагностические сведения о событиях в приложении SignalR.
> 
> В этом разделе изначально был записан с Патрик Флетчера.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4,5
> - SignalR версии 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


При включенной трассировке приложения SignalR создает записи журнала для событий. Можно вести журнал событий от клиента и сервера. Трассировка выполняется на журналы подключение к серверу, поставщик горизонтального масштабирования и события шины сообщений. Трассировка выполняется на события подключения журналы на стороне клиента. В SignalR 2.1 и более поздних версиях трассировки на клиенте записывает полное содержимое сообщения вызова концентратора.

## <a name="contents"></a>Описание

- [Включение трассировки на сервере](#server)

    - [Ведение журнала событий сервера в текстовые файлы](#server_text)
    - [Ведение журнала событий сервера в журнал событий](#server_eventlog)
- [Включение трассировки клиента .NET (приложения для Windows Desktop)](#net_client)

    - [Ведение журнала событий клиента рабочего стола на консоль](#desktop_console)
    - [Ведение журнала событий клиента рабочего стола в текстовый файл](#desktop_text)
- [Включение трассировки в клиентах Windows Phone 8](#phone)

    - [Ведение журнала событий клиента Windows Phone к пользовательскому Интерфейсу](#phone_ui)
    - [Ведение журнала событий клиента Windows Phone в консоли отладки](#phone_debug)
- [Включение трассировки клиента JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Включение трассировки на сервере

Для включения трассировки на сервере в файле конфигурации приложения (App.config или Web.config в зависимости от типа проекта.) Укажите, какие категории событий следует заносить в журнал. В файле конфигурации также укажите, требуется ли в журнал события в текстовый файл, журнал событий Windows или имя пользовательского журнала, используя реализацию [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Категории событий сервера включают следующие типы сообщений:

| Исходный код | Сообщения |
| --- | --- |
| SignalR.SqlMessageBus | Установки поставщика SQL канала сообщений горизонтального масштабирования, операции в базе данных, ошибки и события времени ожидания |
| SignalR.ServiceBusMessageBus | Создание раздела поставщика горизонтального масштабирования шины службы и подписки, ошибок и событий обмена сообщениями |
| SignalR.RedisMessageBus | События подключения, отключения и ошибка поставщика горизонтального масштабирования redis |
| SignalR.ScaleoutMessageBus | Событий сообщений горизонтального масштабирования |
| SignalR.Transports.WebSocketTransport | События подключения, отключения, обмен сообщениями и ошибка транспорт WebSocket |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents транспорта события подключения, отключения, обмен сообщениями и ошибки |
| SignalR.Transports.ForeverFrameTransport | События ForeverFrame транспортного подключения, отключения, обмен сообщениями и ошибки |
| SignalR.Transports.LongPollingTransport | События LongPolling транспортного подключения, отключения, обмен сообщениями и ошибки |
| SignalR.Transports.TransportHeartBeat | События подключения, отключения и keepalive транспорта |
| SignalR.ReflectedHubDescriptorProvider | Концентратор событий обнаружения |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Ведение журнала событий сервера в текстовые файлы

Следующий код показывает, как включить трассировку для каждой категории событий. В этом примере приложение настраивается для регистрации событий в текстовые файлы.

**XML-код сервера для включения трассировки**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

В представленном выше коде `SignalRSwitch` запись указывает [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) используются для отправки в указанном журнале событий. В этом случае оно равно `Verbose` регистрируются означает, что все сообщения отладки и трассировки.

Ниже показано из записи `transports.log.txt` файл с помощью указанного выше файла конфигурации приложения. Он иллюстрирует создание нового соединения, удаленное подключение и события пульс транспорта.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Ведение журнала событий сервера в журнал событий

Чтобы записывать события в журнал событий, а не текстовый файл, изменить значения для записи в `sharedListeners` узла. Ниже показано, как в журнал событий сервера в журнал событий:

**XML-код сервера для ведения журнала событий в журнал событий**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

События регистрируются в журнале приложений и доступны через средство просмотра событий, как показано ниже:

![Средство просмотра событий отображаются журналы SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> При использовании журнала событий, установить **TraceLevel** для **ошибка** для сохранения управляемых число сообщений.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Включение трассировки клиента .NET (приложения для Windows Desktop)

Клиент .NET можно вести журнал в консоль, текстовый файл, или пользовательский журнал, используя реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Включение ведения журнала в клиент .NET, задайте соединение с `TraceLevel` свойства [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) значение и `TraceWriter` свойство допустимому [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) экземпляра.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Ведение журнала событий клиента рабочего стола на консоль

Следующий код C# показано, как регистрация событий в клиент .NET на консоль:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Ведение журнала событий клиента рабочего стола в текстовый файл

Следующий код C# показано, как регистрация событий в клиент .NET в текстовый файл:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Ниже показано из записи `ClientLog.txt` файл с помощью указанного выше файла конфигурации приложения. Показывает клиент, подключающийся к серверу, и вызывается вызов клиентского метода концентратора `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Включение трассировки в клиентах Windows Phone 8

SignalR приложения для приложений Windows Phone используют один и тот же самый клиент .NET как классические приложения, но [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) и записи в файл с [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) недоступны. Вместо этого необходимо создать пользовательскую реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) для трассировки. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Ведение журнала событий клиента Windows Phone к пользовательскому Интерфейсу

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) представлен образец Windows Phone, записывает выходные данные трассировки в [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) с использованием пользовательской [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) реализация вызван `TextBlockWriter`. Этот класс можно найти в **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** проекта. При создании экземпляра `TextBlockWriter`, передайте в текущем [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)и [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) где создаст [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) для трассировки выходные данные:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Выходные данные трассировки затем записываются в новый [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) в [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) передается в:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Ведение журнала событий клиента Windows Phone в консоли отладки

Для отправки выходных данных консоли отладки, а не пользовательский Интерфейс, создайте реализацию [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , записывает в окно отладки и назначьте его подключение к [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) свойства:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Данные трассировки записываются в окно отладки в Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Включение трассировки клиента JavaScript

Чтобы включить ведение журнала клиентского соединения, установите `logging` свойство в объекте подключения перед вызовом метода `start` метод, чтобы установить соединение.

**Клиентский код JavaScript для включения трассировки на консоль браузера (с созданный прокси-сервера)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Клиентский код JavaScript для включения трассировки на консоль браузера (без созданный прокси-сервера)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Если трассировка включена, клиент JavaScript записывает события в консоли браузера. Для доступа к консоли браузера, в разделе [мониторинг транспортов](../getting-started/introduction-to-signalr.md#MonitoringTransports).

На следующем снимке экрана показано клиента SignalR JavaScript с включенной трассировкой. В консоли браузера показано соединение и вызова события концентратора.

![События трассировки SignalR в консоли браузера](enabling-signalr-tracing/_static/image4.png)
