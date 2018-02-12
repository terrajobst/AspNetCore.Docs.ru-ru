---
title: "Поддержка WebSockets в ASP.NET Core"
author: tdykstra
description: "Сведения о начале работы с WebSocket в ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Общие сведения о WebSockets в ASP.NET Core

Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)

Эта статья описывает начало работы с WebSocket в ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям. Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)). Дополнительные сведения см. в разделе [Следующие шаги](#next-steps).


## <a name="prerequisites"></a>Предварительные требования

* ASP.NET Core 1.1 (не запускается в версии 1.0)
* Любая ОС, где работает ASP.NET Core:
  
  * Windows 7/Windows Server 2008 или более поздних версий
  * Linux
  * macOS

* **Исключение**: если ваше приложение выполняется в Windows с использованием IIS или WebListener, нужно использовать:

  * Windows 8/Windows Server 2012 или более поздних версий
  * IIS 8/IIS 8 Express
  * Требуется включить WebSocket в IIS

* Поддерживаемые браузеры см. на странице http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Условия использования

Используйте WebSocket, когда требуется работать непосредственно с подключением через сокет. Например, вам может потребоваться обеспечить наилучшую производительность для игры в режиме реального времени.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет расширенную модель приложения для функций реального времени, однако работает только в ASP.NET, но не в ASP.NET Core. Версия SignalR Core еще разрабатывается. Проследить за ходом разработки можно в [репозитории GitHub для SignalR Core](https://github.com/aspnet/SignalR).

Если вы не хотите ждать SignalR Core, можете использовать WebSocket уже сейчас. Однако вам может потребоваться разработать функции, которые будет предоставлять SignalR, например:

* Поддержка расширенного набора версий браузеров с автоматическим возвратом к альтернативным методам передачи.
* Автоматическое переподключение в случае разрыва соединения.
* Поддержка методов вызова клиента на сервере или наоборот.
* Поддержка масштабирования до нескольких серверов.

## <a name="how-to-use-it"></a>Использование

* Установка пакета [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Настройка ПО промежуточного слоя.
* Принятие запросов WebSocket.
* Отправка и получение сообщений.

### <a name="configure-the-middleware"></a>Настройка ПО промежуточного слоя

Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Можно настроить следующие параметры:

* `KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.
* `ReceiveBufferSize` — размер буфера, используемого для получения данных. Изменение этого параметра может потребоваться лишь опытным пользователям и позволяет настроить производительность с учетом размера данных.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Принятие запросов WebSocket

На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или действии MVC) проверьте, относится ли запрос к WebSocket, и примите его.

Этот пример взят из дальнейшей части метода `Configure`.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.

### <a name="send-and-receive-messages"></a>Отправка и получение сообщений

Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет вам объект [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket). Используйте этот объект для отправки и получения сообщений.

Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo` (здесь это именно метод `Echo`). Код принимает сообщение и сразу отправляет такое же сообщение обратно. Он выполняет этот цикл, пока клиент не закроет соединение. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Если вы принимаете WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается.  После закрытия сокета конвейер развертывается. То есть при принятии WebSocket запрос прекращает продвигаться вперед по конвейеру, как это случается, например, при попадании на действие MVC.  Однако когда вы завершите этот цикл и закроете сокет, запрос продолжит движение по конвейеру.

## <a name="next-steps"></a>Следующие шаги

К этой статье прилагается [пример](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) в виде простого приложения вывода на экран. Он имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер просто перенаправляет клиенту все полученные сообщения. Запустите его из командной строки (он не предназначен для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000. В верхнем левом углу веб-страницы отображается состояние подключения:

![Начальное состояние веб-страницы](websockets/_static/start.png)

Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес.  Введите тестовое сообщение и выберите **Send** (Отправить). После этого выберите **Close Socket** (Закрыть сокет). В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.

![Начальное состояние веб-страницы](websockets/_static/end.png)
