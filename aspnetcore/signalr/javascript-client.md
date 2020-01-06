---
title: ASP.NET Core SignalR клиента JavaScript
author: bradygaster
description: Общие сведения о ASP.NET Core SignalR клиента JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: eaf737642cdbd7ab2b1b5c16538b47a70cddd332
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354700"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>ASP.NET Core SignalR клиента JavaScript

Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)

Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>Установка пакета клиента SignalR

Клиентская библиотека SignalR JavaScript доставляется в виде пакета [NPM](https://www.npmjs.com/) . Если вы используете Visual Studio, запустите `npm install` из **консоли диспетчера пакетов** в корневой папке. Для Visual Studio Code выполните команду из **интегрированного терминала**.

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

## <a name="use-the-opno-locsignalr-javascript-client"></a>Использование SignalR клиента JavaScript

Сослаться на клиент SignalR JavaScript в элементе `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Следующий код создает и запускает соединение. Имя центра не учитывает регистр.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Подключения между источниками

Как правило, Браузеры загружают соединения из того же домена, что и запрошенная страница. Однако бывают случаи, когда требуется подключение к другому домену.

Чтобы предотвратить чтение конфиденциальных данных с другого сайта вредоносным сайтом, [межисточниковые подключения](xref:security/cors) отключены по умолчанию. Чтобы разрешить запрос между источниками, включите его в классе `Startup`.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Методы концентратора вызовов от клиента

Клиенты JavaScript вызывают открытые методы на концентраторах с помощью метода [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) [хубконнектион](/javascript/api/%40aspnet/signalr/hubconnection). Метод `invoke` принимает два аргумента:

* Имя метода концентратора. В следующем примере имя метода в концентраторе — `SendMessage`.
* Любые аргументы, определенные в методе концентратора. В следующем примере имя аргумента — `message`. В примере кода используется синтаксис функции стрелок, который поддерживается в текущих версиях всех основных браузеров, кроме Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Если вы используете службу SignalR Azure в *Бессерверном режиме*, вы не можете вызывать методы концентратора из клиента. Дополнительные сведения см. в [документации по службеSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

Метод `invoke` возвращает [обещание](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)JavaScript. `Promise` разрешается с возвращаемым значением (при наличии), когда метод на сервере возвращает значение. Если метод на сервере вызывает ошибку, `Promise` отклоняется с сообщением об ошибке. Используйте методы `then` и `catch` в `Promise`, чтобы обрабатывались эти случаи (или синтаксис `await`).

Метод `send` возвращает `Promise`JavaScript. `Promise` разрешается, когда сообщение было отправлено на сервер. Если при отправке сообщения произошла ошибка, `Promise` отклоняется с сообщением об ошибке. Используйте методы `then` и `catch` в `Promise`, чтобы обрабатывались эти случаи (или синтаксис `await`).

> [!NOTE]
> Использование `send` не ждет, пока сервер не получит сообщение. Следовательно, невозможно вернуть данные или ошибки с сервера.

## <a name="call-client-methods-from-hub"></a>Вызов методов клиента из концентратора

Чтобы получить сообщения от концентратора, определите метод с помощью метода [on](/javascript/api/%40aspnet/signalr/hubconnection#on) `HubConnection`.

* Имя клиентского метода JavaScript. В следующем примере имя метода — `ReceiveMessage`.
* Аргументы, которые центр передает в метод. В следующем примере значение аргумента — `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Приведенный выше код в `connection.on` выполняется, когда код на стороне сервера вызывает его с помощью метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR определяет, какой клиентский метод вызывать, сопоставляя имя и аргументы метода, определенные в `SendAsync` и `connection.on`.

> [!NOTE]
> Рекомендуется вызвать метод [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) для `HubConnection` после `on`. Это гарантирует, что обработчики будут зарегистрированы до получения сообщений.

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Привязать метод `catch` к концу метода `start` для управления ошибками на стороне клиента. Используйте `console.error` для вывода ошибок в консоль браузера.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Настройте трассировку журнала на стороне клиента, передав средства ведения журнала и тип события в журнал, когда соединение установлено. Сообщения записываются с указанным уровнем ведения журнала и выше. Доступны следующие уровни ведения журнала:

* `signalR.LogLevel.Error` &ndash; сообщения об ошибках. Журналы `Error` только сообщения.
* `signalR.LogLevel.Warning` &ndash; предупреждающие сообщения о возможных ошибках. Регистрирует `Warning`и `Error` сообщения.
* `signalR.LogLevel.Information` &ndash; сообщения о состоянии без ошибок. Регистрирует `Information`, `Warning`и `Error` сообщений.
* `signalR.LogLevel.Trace` &ndash; сообщения трассировки. Заносит в журнал все данные, включая транспорт данных между концентратором и клиентом.

Чтобы настроить уровень ведения журнала, используйте метод [конфигурелоггинг](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) в [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) . Сообщения записываются в консоль браузера.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Повторное подключение клиентов

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Автоматическое повторное подключение

Клиент JavaScript для SignalR можно настроить для автоматического повторного подключения с помощью метода `withAutomaticReconnect` в [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). По умолчанию оно не будет автоматически восстановлено.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Без параметров `withAutomaticReconnect()` настраивает клиент на ожидание 0, 2, 10 и 30 секунд соответственно, прежде чем пытаться выполнить каждую попытку повторного подключения, остановить после четырех неудачных попыток.

Перед запуском всех попыток повторного подключения `HubConnection` перейдет в состояние `HubConnectionState.Reconnecting` и порождает его `onreconnecting`ные обратные вызовы вместо перехода в состояние `Disconnected` и запуска его `onclose`ные обратные вызовы, такие как `HubConnection` без настроенного автоматического повторного подключения. Это дает возможность предупредить пользователей о потере подключения и отключении элементов пользовательского интерфейса.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Если клиент успешно повторно подключится в течение первых четырех попыток, `HubConnection` вернется в состояние `Connected` и порождает его `onreconnected`ные обратные вызовы. Это дает возможность информировать пользователей о повторном установлении соединения.

Так как соединение выглядит только на сервере, новое `connectionId` будет предоставлено в ответный вызов `onreconnected`.

> [!WARNING]
> Параметр `connectionId` обратного вызова `onreconnected` не определен, если `HubConnection` был настроен на [пропуск согласования](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` не настраивает `HubConnection` для повторного выполнения начальных ошибок запуска, поэтому ошибки запуска должны быть обработаны вручную:

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

Если клиент не будет успешно подключаться в течение первых четырех попыток, `HubConnection` перейдет в состояние `Disconnected` и порождает его обратные вызовы [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) . Это дает возможность информировать пользователей о том, что подключение было безвозвратно утрачено, и рекомендует обновить страницу:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Чтобы настроить настраиваемое число попыток повторного подключения перед отключением или изменением времени повторного подключения, `withAutomaticReconnect` принимает массив чисел, представляющих задержку в миллисекундах, прежде чем начинать каждую попытку повторного подключения.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

В предыдущем примере выполняется настройка `HubConnection` для начала попытки повторного подключения сразу после потери соединения. Это также справедливо для конфигурации по умолчанию.

Если первая попытка повторного подключения завершается неудачно, вторая попытка повторного подключения также начнется немедленно, а не подождать 2 секунды, как в конфигурации по умолчанию.

Если вторая попытка повторного подключения завершается неудачно, третья попытка повторного подключения начнется через 10 секунд, что опять же, как и конфигурация по умолчанию.

Затем пользовательское поведение снова рассходится от поведения по умолчанию, останавливаясь после третьей попытки повторного подключения, вместо того, чтобы предпринимать попытку повторного подключения еще через 30 секунд, как в конфигурации по умолчанию.

Если требуется еще более контролировать время и число попыток автоматического повторного подключения, `withAutomaticReconnect` принимает объект, реализующий интерфейс `IRetryPolicy`, который имеет один метод с именем `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` принимает один аргумент с типом `RetryContext`. `RetryContext` имеет три свойства: `previousRetryCount`, `elapsedMilliseconds` и `retryReason`, которые являются `number`, `number` и `Error` соответственно. Перед первой попыткой повторного подключения оба `previousRetryCount` и `elapsedMilliseconds` будут равны нулю, а `retryReason` будет являться ошибкой, вызвавшей потерю соединения. После каждой неудачной попытки повторного выполнения `previousRetryCount` будет увеличиваться на единицу, `elapsedMilliseconds` будет обновлена с учетом времени, затраченного на повторное подключение к прошлому в миллисекундах, а `retryReason` будет являться ошибкой, которая привела к сбою последней попытки повторного подключения.

`nextRetryDelayInMilliseconds` должен возвращать число, представляющее время ожидания (в миллисекундах) перед следующей попыткой повторного подключения, или `null`, если `HubConnection` следует переустановить повторное подключение.

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
        }
    })
    .build();
```

Кроме того, можно написать код, который будет повторно подключаться к клиенту вручную, как показано в [подсоединении вручную](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Повторное подключение вручную

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> До 3,0 клиент JavaScript для SignalR не выполняет автоматическое повторное подключение. Необходимо написать код, который будет повторно подключаться к клиенту вручную.

::: moniker-end

В следующем коде показан типичный способ повторного подключения вручную:

1. Для запуска подключения создается функция (в данном случае функция `start`).
1. Вызовите функцию `start` в обработчике событий `onclose` соединения.

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
* [Документация по Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
