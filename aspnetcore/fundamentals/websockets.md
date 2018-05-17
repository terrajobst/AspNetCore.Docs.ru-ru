---
title: Поддержка WebSockets в ASP.NET Core
author: tdykstra
description: Сведения о начале работы с WebSocket в ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: e744ab5b81ff85f48edb012a86b55003cc74929c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="websockets-support-in-aspnet-core"></a>Поддержка WebSockets в ASP.NET Core

Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)

Эта статья описывает начало работы с WebSocket в ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям. Он используется в приложениях, где нужна быстрая связь в режиме реального времени, например в чатах, панелях мониторинга и играх.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)). Дополнительные сведения см. в разделе [Следующие шаги](#next-steps).

## <a name="prerequisites"></a>Предварительные требования

* ASP.NET Core 1.1 или более поздней версии
* Любая ОС с поддержкой ASP.NET Core:
  
  * Windows 7/Windows Server 2008 или более поздних версий
  * Linux
  * macOS
  
* Если приложение выполняется в Windows с IIS:

  * Windows 8/Windows Server 2012 или более поздних версий
  * IIS 8/IIS 8 Express
  * В службах IIS необходимо включить WebSockets (см. раздел [Поддержка IIS и IIS Express](#iisiis-express-support).)
  
* Если приложение выполняется в [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8/Windows Server 2012 или более поздних версий

* Список поддерживаемых обозревателей см. на странице https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Условия использования WebSocket

Используйте WebSocket для работы с подключением через сокет напрямую. Например, можно использовать WebSocket для оптимальной производительности игры в режиме реального времени.

[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет расширенную модель приложения для функций реального времени, однако работает только в ASP.NET 4.x, но не в ASP.NET Core. Версия SignalR в ASP.NET Core будет выпущена в ASP.NET Core 2.1. См. [Планирование высокого уровня в ASP.NET Core 2.1](https://github.com/aspnet/Announcements/issues/288).

До выпуска SignalR Core можно использовать WebSocket. Но функции, предоставляемые SignalR, должны предоставляться и поддерживаться разработчиком. Пример:

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

Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Можно настроить следующие параметры:

* `KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.
* `ReceiveBufferSize` — размер буфера, используемого для получения данных. Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Принятие запросов WebSocket

На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или действии MVC) проверьте, относится ли запрос к WebSocket, и примите его.

Следующий пример взят из дальнейшей части метода `Configure`:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.

### <a name="send-and-receive-messages"></a>Отправка и получение сообщений

Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет объект [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Используйте объект `WebSocket` для отправки и получения сообщений.

Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo`. Код принимает сообщение и сразу отправляет такое же сообщение обратно. Сообщения отправляются и получаются циклически, пока клиент не закроет подключение:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Если вы принимаете подключение WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается. После закрытия сокета конвейер развертывается. То есть запрос перестает перемещаться по конвейеру после принятия WebSocket. После завершения цикла и закрытия сокета запрос возвращается в конвейер.

## <a name="iisiis-express-support"></a>Поддержка служб IIS/IIS Express

Windows Server 2012 или более поздней версии и Windows 8 или более поздней версии с IIS и IIS Express 8 или более поздней версии поддерживают протокол WebSocket.

Чтобы включить поддержку протокола WebSocket в Windows Server 2012 или более поздней версии:

1. В меню **Управление** запустите мастер **Добавить роли и компоненты** или в окне **Диспетчер серверов** щелкните соответствующую ссылку.
1. Выберите **Установка ролей или компонентов**. Выберите **Далее**.
1. Выберите подходящий сервер (по умолчанию выбирается локальный сервер). Выберите **Далее**.
1. Разверните **Веб-сервер (IIS)** в дереве **Роли**, разверните **Веб-сервер**, а затем **Разработка приложений**.
1. Выберите **протокол WebSocket**. Выберите **Далее**.
1. Если дополнительные функции не требуются, нажмите **Далее**.
1. Нажмите кнопку **Установить**.
1. По завершении установки выберите **Закрыть**, чтобы выйти из мастера.

Чтобы включить поддержку протокола WebSocket в Windows 8 или более поздней версии:

1. Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).
1. Откройте следующе узлы: **IIS** > **Службы Интернета** > **Компоненты разработки приложений**.
1. Выберите компонент **Протокол WebSocket**. Нажмите кнопку **ОК**.

**Отключите WebSocket при использовании socket.io на node.js**

Если используется поддержка WebSocket в [socket.io](https://socket.io/) на [Node.js](https://nodejs.org/), отключите модуль WebSocket IIS по умолчанию с помощью элемента `webSocket` в *web.config* или *applicationHost.config*. Если не выполнить этот шаг, модуль IIS WebSocket попытается обработать соединение WebSocket, а не Node.js и приложение.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Следующие шаги

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) в этой статье — это эхо-приложение. Оно имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер перенаправляет все полученные сообщения обратно клиенту. Запустите приложение из командной строки (оно не предназначено для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000. В верхнем левом углу веб-страницы отображается состояние подключения:

![Начальное состояние веб-страницы](websockets/_static/start.png)

Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес. Введите тестовое сообщение и выберите **Send** (Отправить). После этого выберите **Close Socket** (Закрыть сокет). В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.

![Начальное состояние веб-страницы](websockets/_static/end.png)
