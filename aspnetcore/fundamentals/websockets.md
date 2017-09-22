---
title: "Поддержка WebSockets в ASP.NET Core"
author: tdykstra
description: "Что такое WebSockets поддержки в ASP.NET Core и способ его использования."
keywords: ASP.NET Core, WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 8a6b5cc8ca8ac17f0e4c5b23f20013130cd472c8
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Общие сведения о WebSockets в ASP.NET Core

По [Tom Dykstra](https://github.com/tdykstra) и [Эндрю Stanton сиделка](https://github.com/anurse)

В этой статье объясняется, как начало работы с WebSockets в ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, позволяющий двусторонний обмен данными постоянно каналы через TCP-подключений. Он используется для приложений, например чат, каналам биржевых котировок, игры, в любое место в режиме реального времени функциональные возможности веб-приложения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample). В разделе [дальнейшие действия](#next-steps) Дополнительные сведения.


## <a name="prerequisites"></a>Предварительные требования

* ASP.NET Core 1.1 (не работает на 1.0)
* Любой ОС, работающей в среде ASP.NET Core:
  
  * Windows 7 или Windows Server 2008 и более поздних версий
  * Linux
  * MacOS

* **Исключение**: Если ваше приложение запускается в Windows с помощью IIS, или с WebListener, необходимо использовать:

  * Windows 8 и Windows Server 2012 или более поздней версии
  * IIS 8 / IIS 8 Express
  * WebSocket необходимо включить в службах IIS

* Для поддерживаемых браузерах см. в разделе http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Условия использования

Используйте WebSocket, когда требуется работать непосредственно с подключения к сокету. Например может потребоваться наилучшую производительность для игр в режиме реального времени.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет всестороннее модель приложения для функций в режиме реального времени, но он работает только на ASP.NET, не ASP.NET Core. Версия ядра SignalR находится в стадии разработки; ход его выполнения, разделе [репозитории GitHub для SignalR Core](https://github.com/aspnet/SignalR).

Если не нужно ждать SignalR Core, вы можете использовать WebSocket прямо сейчас. Но возможно, потребуется разрабатывать средства обеспечения SignalR бы, например:

* Поддержка более широкий спектр версии браузеров с помощью автоматического возврата к транспорта альтернативные методы.
* Автоматическое переподключение разрыва подключения.
* Поддержка клиентов, вызывающих методы на сервере или наоборот.
* Поддержка масштабирования на несколько серверов.

## <a name="how-to-use-it"></a>Способ его использования

* Установка [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) пакета.
* Настройки по промежуточного слоя.
* Принимает запросы WebSocket.
* Отправлять и получать сообщения.

### <a name="configure-the-middleware"></a>Настройки по промежуточного слоя

Добавить по промежуточного слоя WebSockets в `Configure` метод `Startup` класса.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Можно настроить следующие параметры:

* `KeepAliveInterval`-Как часто следует отправлять клиенту, чтобы убедиться, что учетные записи-посредники сохранить открытым подключение «ping» кадры.
* `ReceiveBufferSize`— Размер буфера, используемого для получения данных. Только опытным пользователям может потребоваться изменить его, для настройки производительности на основе размера свои данные.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Принимать запросы WebSocket

Где-либо более поздней версии в жизненном цикле запроса (Далее в `Configure` метод или действие MVC, например) Проверьте, является ли запрос WebSocket и принять запрос WebSocket.

Этот пример взят из более поздней версии в `Configure` метод.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Запрос WebSocket могут приходить в любой URL-адрес, но этот пример кода принимает только запросы для `/ws`.

### <a name="send-and-receive-messages"></a>Отправка и получение сообщений

`AcceptWebSocketAsync` Метод обновляет TCP-соединение до соединения WebSocket и дает [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) объекта. Используйте объект WebSocket для отправки и получения сообщений.

Код, приведенный выше, которая принимает запрос WebSocket передает `WebSocket` объект `Echo` метод; вот `Echo` метод. Код получает сообщение и немедленно отправляет одно сообщение. Он остается в состоянии цикла, это сделать, пока клиент не закроет подключение. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Если принять WebSocket до начала этого цикла, завершает конвейера по промежуточного слоя.  После закрытия сокета, освобождает конвейера. То есть запрос останавливается, продвижение вперед в конвейере при принятии WebSocket, как это будет при попадании действие MVC, например.  Однако по завершении этого цикла и закрыть сокет, запрос выполняется резервное копирование конвейера.

## <a name="next-steps"></a>Следующие шаги

[Образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , сопровождающий это статья — это простые эхо приложение. Он имеет веб-страницы, которая устанавливает соединения WebSocket, и сервер просто повторно отправляет клиенту все сообщения, которые он получает. Запустите его из командной строки (она не включена для запуска из Visual Studio с IIS Express) и перейдите к http://localhost: 5000. Веб-странице показано состояние подключения в верхнем левом углу.

![Начальное состояние веб-страницы](websockets/_static/start.png)

Выберите **Connect** отправить запрос WebSocket показано URL-адрес.  Введите тестовое сообщение и выберите **отправки**. После этого выберите **закрыть сокет**. **Журнала связи** раздел сообщает каждого открытия, отправки и действие закрытия, как это происходит.

![Начальное состояние веб-страницы](websockets/_static/end.png)
