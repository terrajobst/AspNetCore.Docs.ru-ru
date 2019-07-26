---
title: Клиент JavaScript ASP.NET Core SignalR
author: bradygaster
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/28/2019
uid: signalr/javascript-client
ms.openlocfilehash: f314e1fe0ef0ea73a28b332404a09f2956524132
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412381"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Клиент JavaScript ASP.NET Core SignalR

Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)

Клиентская библиотека JavaScript ASP.NET Core SignalR позволяет разработчикам вызывать код концентратора на стороне сервера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Установка клиентского пакета SignalR

Клиентская библиотека JavaScript SignalR поставляется в виде пакета [NPM](https://www.npmjs.com/) . Если вы используете Visual Studio, запустите `npm install` из **консоли диспетчера пакетов** , находясь в корневой папке. Для Visual Studio Code выполните команду из **интегрированного терминала**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

npm устанавливает содержимое пакета в *node_modules\\@microsoft\signalr\dist\browser* папки. Создайте новую папку с именем *SignalR* в папке *wwwroot\\lib* . Скопируйте файл *SignalR. js* в папку *ввврут\либ\сигналр*

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm устанавливает содержимое пакета в *node_modules\\@aspnet\signalr\dist\browser* папки. Создайте новую папку с именем *SignalR* в папке *wwwroot\\lib* . Скопируйте файл *SignalR. js* в папку *ввврут\либ\сигналр*

::: moniker-end

## <a name="use-the-signalr-javascript-client"></a>Использовать клиент JavaScript SignalR

Ссылка на клиент JavaScript SignalR в `<script>` элементе.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Следующий код создает и запускает соединение. Имя центра не учитывает регистр.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Подключения между источниками

Как правило, Браузеры загружают соединения из того же домена, что и запрошенная страница. Однако бывают случаи, когда требуется подключение к другому домену.

Чтобы предотвратить чтение конфиденциальных данных с другого сайта вредоносным сайтом, [межисточниковые подключения](xref:security/cors) отключены по умолчанию. Чтобы разрешить запрос между источниками, включите его в `Startup` классе.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Методы концентратора вызовов от клиента

Клиенты JavaScript вызывают открытые методы на концентраторах с помощью метода [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) [хубконнектион](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере имя метода в концентраторе — `SendMessage`.
* Любые аргументы, определенные в методе концентратора. В следующем примере имя аргумента — `message`. В примере кода используется синтаксис функции стрелок, который поддерживается в текущих версиях всех основных браузеров, кроме Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Если вы используете службу Azure SignalR в бессерверном *режиме*, вы не можете вызывать методы концентратора из клиента. Дополнительные сведения см. в [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

Метод возвращает обещание JavaScript. [](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) `invoke` `Promise` Разрешение разрешается с возвращаемым значением (если таковое имеется), когда метод на сервере возвращает значение. Если метод на сервере вызывает ошибку, объект `Promise` отклоняется с сообщением об ошибке. `catch` Используйтеметоды`Promise` идля`await` самого себя, чтобы обрабатывали такие случаи (или синтаксис). `then`

Метод возвращает код JavaScript `Promise`. `send` `Promise` Разрешение разрешается, когда сообщение было отправлено на сервер. Если при отправке сообщения произошла ошибка, объект `Promise` отклоняется с сообщением об ошибке. `catch` Используйтеметоды`Promise` идля`await` самого себя, чтобы обрабатывали такие случаи (или синтаксис). `then`

> [!NOTE]
> Использование `send` не ждет, пока сервер не получит сообщение. Следовательно, невозможно вернуть данные или ошибки с сервера.

## <a name="call-client-methods-from-hub"></a>Вызов методов клиента из концентратора

Чтобы получить сообщения от концентратора, определите метод с помощью метода [On](/javascript/api/%40aspnet/signalr/hubconnection#on) класса `HubConnection`.

* Имя клиентского метода JavaScript. В следующем примере имя метода — `ReceiveMessage`.
* Аргументы, которые центр передает в метод. В следующем примере аргумент имеет `message`значение.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Приведенный выше код `connection.on` выполняется, когда код на стороне сервера вызывает его с помощью метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR определяет, какой клиентский метод следует вызывать, сопоставляя имя и аргументы метода, определенные `SendAsync` в `connection.on`и.

> [!NOTE]
> Рекомендуется вызвать метод [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) для `HubConnection` after `on`. Это гарантирует, что обработчики будут зарегистрированы до получения сообщений.

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

`start` Привязать `catch` метод к концу метода для управления ошибками на стороне клиента. Используется `console.error` для вывода ошибок в консоль браузера.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Настройте трассировку журнала на стороне клиента, передав средства ведения журнала и тип события в журнал, когда соединение установлено. Сообщения записываются с указанным уровнем ведения журнала и выше. Доступны следующие уровни ведения журнала:

* `signalR.LogLevel.Error`&ndash; Сообщения об ошибках. Записывает `Error` только сообщения.
* `signalR.LogLevel.Warning`&ndash; Предупреждающие сообщения о возможных ошибках. Журналы `Warning` и`Error` сообщения.
* `signalR.LogLevel.Information`&ndash; Сообщения о состоянии без ошибок. Журналы `Information`, `Warning` и`Error` сообщения.
* `signalR.LogLevel.Trace`&ndash; Сообщения трассировки. Заносит в журнал все данные, включая транспорт данных между концентратором и клиентом.

Чтобы настроить уровень ведения журнала, используйте метод [конфигурелоггинг](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) в [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) . Сообщения записываются в консоль браузера.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Повторное подключение клиентов

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Автоматическое повторное подключение

Клиент JavaScript для SignalR можно настроить для автоматического повторного подключения с помощью `withAutomaticReconnect` метода в [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). По умолчанию оно не будет автоматически восстановлено.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Без каких бы то `withAutomaticReconnect()` ни было параметров, настраивает клиент на ожидание 0, 2, 10 и 30 секунд соответственно, прежде чем пытаться выполнить каждую попытку повторного подключения, останавливая после четырех неудачных попыток.

Перед запуском всех попыток `HubConnection` повторного подключения компонент переходит `HubConnectionState.Reconnecting` в состояние и срабатывает `onreconnecting` на `Disconnected` обратные вызовы вместо перехода в состояние и запуска своих `onclose` обратных вызовов, таких как `HubConnection`без настроенного автоматического повторного подключения. Это дает возможность предупредить пользователей о потере подключения и отключении элементов пользовательского интерфейса.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Если клиент успешно повторно подключится в течение первых четырех попыток, `HubConnection` компонент вернется `Connected` к состоянию и порождает его `onreconnected` обратные вызовы. Это дает возможность информировать пользователей о повторном установлении соединения.

Так как соединение выглядит совершенно не так, как и сервер, `connectionId` `onreconnected` для обратного вызова будет предоставлен новый объект.

> [!WARNING]
> Параметр обратного вызова будет неопределенным `HubConnection` , если был настроен на [пропуск согласования](xref:signalr/configuration#configure-client-options). `onreconnected` `connectionId`

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()`не настраивается `HubConnection` для повтора начальных сбоев запуска, поэтому ошибки запуска должны быть обработаны вручную:

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

Если клиент не `HubConnection` будет успешно подключаться в течение первых четырех попыток, компонент перейдет `Disconnected` в состояние и порождает его обратные вызовы OnClose. [](/javascript/api/%40aspnet/signalr/hubconnection#onclose) Это дает возможность информировать пользователей о том, что подключение было безвозвратно утрачено, и рекомендует обновить страницу:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Чтобы настроить настраиваемое число попыток повторного подключения перед отключением или изменением времени повторного подключения, принимает `withAutomaticReconnect` массив чисел, представляющих задержку в миллисекундах, прежде чем начинать каждую попытку повторного подключения.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

В предыдущем примере настраивается `HubConnection` для запуска попытки повторного подключения сразу после потери соединения. Это также справедливо для конфигурации по умолчанию.

Если первая попытка повторного подключения завершается неудачно, вторая попытка повторного подключения также начнется немедленно, а не подождать 2 секунды, как в конфигурации по умолчанию.

Если вторая попытка повторного подключения завершается неудачно, третья попытка повторного подключения начнется через 10 секунд, что опять же, как и конфигурация по умолчанию.

Затем пользовательское поведение снова рассходится от поведения по умолчанию, останавливаясь после третьей попытки повторного подключения, вместо того, чтобы предпринимать попытку повторного подключения еще через 30 секунд, как в конфигурации по умолчанию.

Если требуется даже больший контроль над временем и числом попыток автоматического повторного подключения, `withAutomaticReconnect` принимает объект, `IRetryPolicy` реализующий интерфейс, который имеет единственный метод с именем `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds`принимает один аргумент с типом `RetryContext`. `elapsedMilliseconds` Имееттри`Error` свойства `previousRetryCount`: `retryReason` , и, а — ,`number` и соответственно. `number` `RetryContext` Перед первой попыткой повторного подключения обе `previousRetryCount` и `elapsedMilliseconds` будут равны нулю, и `retryReason` будет являться ошибкой, вызвавшей потерю соединения. После каждой неудачной попытки `previousRetryCount` повтора значение будет увеличено на единицу, `elapsedMilliseconds` будет обновлено с учетом времени, затраченного на повторное подключение к этому моменту в `retryReason` миллисекундах, и будет являться ошибкой, вызвавшей Последнее повторение попытки повторного подключения. Cчетчик.

`nextRetryDelayInMilliseconds`должен возвращать число, представляющее число миллисекунд ожидания перед следующей попыткой повторного подключения, или `null` значение, `HubConnection` если должно быть прервано повторное подключение.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
          if (retryContext.elapsedMilliseconds < 60000) {
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

Кроме того, можно написать код, который будет повторно подключаться к клиенту вручную, как показано в подсоединении [вручную](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Повторное подключение вручную

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> До 3,0 клиент JavaScript для SignalR не переключается автоматически. Необходимо написать код, который будет повторно подключаться к клиенту вручную.

::: moniker-end

В следующем коде показан типичный способ повторного подключения вручную:

1. Для запуска подключения создается функция (в данном `start` случае это функция).
1. `onclose` Вызовите `start` функцию в обработчике событий соединения.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

В реальной реализации будет использоваться экспоненциальная отсрочка или повторная попытка заданное число раз.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Справочник по API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Учебник по JavaScript](xref:tutorials/signalr)
* [Руководство по пакету и TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Запросы между источниками (CORS)](xref:security/cors)
* [Документация, посвященная серверу службы Azure SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config)
