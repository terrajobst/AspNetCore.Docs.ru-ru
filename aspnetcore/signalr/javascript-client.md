---
title: Клиент ASP.NET Core SignalR JavaScript
author: tdykstra
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: cd64a65889227d84615768bc3d8fddcd362fbba4
ms.sourcegitcommit: eef99d14d96dc8c3c1bb0e2c4cb14da152f8a952
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2018
ms.locfileid: "53022483"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Клиент ASP.NET Core SignalR JavaScript

Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)

Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Установка пакета клиента SignalR

Клиентская библиотека SignalR JavaScript предоставляется в виде [npm](https://www.npmjs.com/) пакета. Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке. Для Visual Studio Code, выполните команду из **интегрированный терминал**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm устанавливает содержимое пакета в *node_modules\\@aspnet\signalr\dist\browser* папки. Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки. Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.

## <a name="use-the-signalr-javascript-client"></a>Использование клиента SignalR JavaScript

Сослаться на клиентский SignalR JavaScript в `<script>` элемент.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Следующий код создает и запускает подключение. Имя концентратора регистр не учитывается.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

### <a name="cross-origin-connections"></a>Независимо от источника соединения

Как правило браузеры нагрузки подключений из тому же домену, что запрошенной страницы. Тем не менее существуют случаи, когда требуется соединение с другим доменом.

Для предотвращения чтения конфиденциальных данных с другого сайта, вредоносный сайт [подключения независимо от источника](xref:security/cors) отключены по умолчанию. Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

Клиенты JavaScript вызывать открытые методы концентраторы через [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) метод [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере, является имя метода на концентраторе `SendMessage`.
* Все аргументы, определенный в методе концентратора. В следующем примере, является имя аргумента `message`.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Для получения сообщений от концентратора, определение метода с помощью [на](/javascript/api/%40aspnet/signalr/hubconnection#on) метод `HubConnection`.

* Имя метода клиента JavaScript. В следующем примере, является имя метода `ReceiveMessage`.
* Аргументы концентратор передает в метод. В следующем примере значение аргумента равно `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Предыдущий код в `connection.on` выполняется, когда серверный код вызывает ее с помощью [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) метод.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR определяют, какой метод клиента для вызова, сопоставляя имя метода и аргументы, определенные в `SendAsync` и `connection.on`.

> [!NOTE]
> Рекомендуется, вызовите [запустить](/javascript/api/%40aspnet/signalr/hubconnection#start) метод `HubConnection` после `on`. Это гарантирует, что зарегистрированных обработчиков до получения сообщения.

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Цепочка `catch` метод в конец `start` метод для обработки ошибок на стороне клиента. Используйте `console.error` для ошибок вывода для консоли браузера.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=43-45)]

Настройка трассировки журнала на стороне клиента, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение. Выводятся сообщения с уровнем журнала и более поздних версий. Ниже перечислены уровни доступных журналов:

* `signalR.LogLevel.Error` &ndash; Сообщения об ошибках. Журналы `Error` только сообщения.
* `signalR.LogLevel.Warning` &ndash; Предупреждающие сообщения об ошибках. Журналы `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Information` &ndash; Сообщения о состоянии без ошибок. Журналы `Information`, `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Trace` &ndash; Сообщения трассировки. В журнал все события, в том числе данных, перемещаемая между центром и клиентом.

Используйте [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) метод [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) настроить уровень ведения журнала. Сообщения записываются в консоли браузера.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Повторное подключение клиентов

Клиент JavaScript для SignalR не повторное соединение производится автоматически. Необходимо написать код, который позволит приложению повторно подключиться клиент вручную. Следующий код демонстрирует подход обычно повторного подключения:

1. Функции (в этом случае `start` функции) создается при запуске подключения.
1. Вызовите `start` функции в соединении `onclose` обработчик событий.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Реальную реализацию будут использовать стратегию экспоненциальной отсрочки или повторите указанное число раз, прежде чем прекратить. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Справочник по API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Учебник по JavaScript](xref:tutorials/signalr)
* [Руководство по WebPack и TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Запросов о происхождении (CORS)](xref:security/cors)
