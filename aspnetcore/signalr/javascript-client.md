---
title: Клиент ASP.NET Core SignalR JavaScript
author: bradygaster
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: f1f072e63928502fa1bad62e808ff035e57f2fd3
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983017"
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

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Независимо от источника соединения

Как правило браузеры нагрузки подключений из тому же домену, что запрошенной страницы. Тем не менее существуют случаи, когда требуется соединение с другим доменом.

Для предотвращения чтения конфиденциальных данных с другого сайта, вредоносный сайт [подключения независимо от источника](xref:security/cors) отключены по умолчанию. Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

Клиенты JavaScript вызывать открытые методы концентраторы через [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) метод [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере, является имя метода на концентраторе `SendMessage`.
* Все аргументы, определенный в методе концентратора. В следующем примере, является имя аргумента `message`. В примере кода используется синтаксис функции со стрелкой, которая поддерживается в текущих версиях всех основных браузерах за исключением Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Если вы используете службу Azure SignalR в *бессерверной режим*, нельзя вызывать методы концентратора клиента. Дополнительные сведения см. в разделе [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

`invoke` Метод возвращает JavaScript [обещание](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise). `Promise` Разрешается с возвращаемым значением (если таковые имеются) при возвращении метода на сервере. Если на сервере метод вызывает ошибку, `Promise` отклоняется с сообщением об ошибке. Используйте `then` и `catch` методы `Promise` сам для обработки таких случаев (или `await` синтаксиса).

`send` Метод возвращает JavaScript `Promise`. `Promise` Разрешается, когда сообщение было отправлено на сервер. Если возникает ошибка отправки сообщений и `Promise` отклоняется с сообщением об ошибке. Используйте `then` и `catch` методы `Promise` сам для обработки таких случаев (или `await` синтаксиса).

> [!NOTE]
> С помощью `send` не приходится ждать сервер получил сообщение. Следовательно он не поддерживается для возвращения данных или ошибки с сервера.

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

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Настройка трассировки журнала на стороне клиента, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение. Выводятся сообщения с уровнем журнала и более поздних версий. Ниже перечислены уровни доступных журналов:

* `signalR.LogLevel.Error` &ndash; Сообщения об ошибках. Журналы `Error` только сообщения.
* `signalR.LogLevel.Warning` &ndash; Предупреждающие сообщения об ошибках. Журналы `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Information` &ndash; Сообщения о состоянии без ошибок. Журналы `Information`, `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Trace` &ndash; Сообщения трассировки. В журнал все события, в том числе данных, перемещаемая между центром и клиентом.

Используйте [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) метод [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) настроить уровень ведения журнала. Сообщения записываются в консоли браузера.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Повторное подключение клиентов

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Автоматическое переподключение

Клиент JavaScript для SignalR можно настроить для автоматического подключения с помощью `withAutomaticReconnect` метод [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Он не будет повторное соединение производится автоматически по умолчанию.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Без параметров `withAutomaticReconnect()` служит для настройки клиента в течение 0, 2, 10 и 30 секунд соответственно, перед попыткой попытками повторного подключения после четырех неудачных попыток.

Прежде чем выполнять любые попытки повторного подключения `HubConnection` будет переход на `HubConnectionState.Reconnecting` состояния и запускать его `onreconnecting` обратные вызовы вместо переход к `Disconnected` состояния и активации его `onclose` обратные вызовы, такие как `HubConnection`настроена без автоматического повторного подключения. Это дает возможность предупредить пользователей о том, что соединение было потеряно и отключать элементы пользовательского интерфейса.

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

Если клиент успешном повторном подключении в первые четыре попытки, `HubConnection` перейдет к `Connected` состояния и запускать его `onreconnected` обратные вызовы. Это дает возможность для информирования пользователей, подключение будет восстановлено.

Так как подключение выглядит совершенно новое на сервер новый `connectionId` будет предоставляться `onreconnected` обратного вызова.

> [!WARNING]
> `onreconnected` Обратного вызова `connectionId` параметра будет неопределенным, если `HubConnection` был настроен для [пропустить согласования](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` не настроить `HubConnection` попытки при сбоях начальной загрузки, поэтому при сбоях загрузки нужно обрабатывать вручную:

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

Если клиент не повторного подключения успешно в первые четыре попытки, `HubConnection` будет переход к `Disconnected` состояния и запускать его [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) обратные вызовы. Это дает возможность для информирования пользователей, соединение постоянно потерян и рекомендует обновить страницу:

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

Чтобы настроить настраиваемое количество попыток повторного подключения до отключения или повторного подключения этот интервал, `withAutomaticReconnect` принимает массив чисел, представляющий задержку в миллисекундах для ожидания перед началом каждой попытки повторного подключения.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

В предыдущем примере настраивается `HubConnection` запустить попытки повторных подключений сразу же после потери связи. Это справедливо также для настройки по умолчанию.

В случае с первой попытки повторного подключения со второй попытки повторного подключения будут также применены немедленно вместо ожидания 2 секунды, как в конфигурации по умолчанию.

В случае со второй попытки повторного подключения третьей попытки повторного подключения начнется через 10 секунд и снова станет как конфигурацию по умолчанию.

Пользовательское поведение затем еще раз расходится со всеми поведение по умолчанию путем остановки после сбоя вместо того, один попытки третий повторное соединение более повторного подключения попытки еще на 30 секунд, как в конфигурации по умолчанию.

Если вы хотите еще лучше контролировать время и число автоматическое переподключение попыток, `withAutomaticReconnect` принимает объект, реализующий интерфейс `IReconnectPolicy` интерфейс, который содержит один метод с именем `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` принимает два аргумента: `previousRetryCount` и `elapsedMilliseconds`, которые являются оба числа. Перед первой попытки повторного подключения оба `previousRetryCount` и `elapsedMilliseconds` будут равны нулю. После каждой попытки повтора неудачных `previousRetryCount` увеличивается на единицу и `elapsedMilliseconds` будут обновлены в соответствии с время, затраченное на повторное подключение в данный момент в миллисекундах.

`nextRetryDelayInMilliseconds` должен возвращать либо число, представляющее количество миллисекунд ожидания перед следующей попыткой повторного подключения или `null` Если `HubConnection` должна прекратиться, повторное подключение.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

Кроме того, можно написать код, который позволит приложению повторно подключиться клиент вручную, как показано в [вручную переподключить](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Вручную переподключить

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> До версии 3.0 клиент JavaScript для SignalR не повторное соединение производится автоматически. Необходимо написать код, который позволит приложению повторно подключиться клиент вручную.

::: moniker-end

Следующий код демонстрирует подход обычно ручного повторного подключения:

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
* [SignalR бессерверной документация по службе Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
