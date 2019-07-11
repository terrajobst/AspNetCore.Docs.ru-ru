---
title: Клиент ASP.NET Core SignalR JavaScript
author: bradygaster
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 8b645304b597db0c37fb9cd8814c493ca1c6ee62
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814966"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="825ef-103">Клиент ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="825ef-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="825ef-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="825ef-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="825ef-105">Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="825ef-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="825ef-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="825ef-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="825ef-107">Установка пакета клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="825ef-107">Install the SignalR client package</span></span>

<span data-ttu-id="825ef-108">Клиентская библиотека SignalR JavaScript предоставляется в виде [npm](https://www.npmjs.com/) пакета.</span><span class="sxs-lookup"><span data-stu-id="825ef-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="825ef-109">Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="825ef-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="825ef-110">Для Visual Studio Code, выполните команду из **интегрированный терминал**.</span><span class="sxs-lookup"><span data-stu-id="825ef-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="825ef-111">npm устанавливает содержимое пакета в *node_modules\\@aspnet\signalr\dist\browser* папки.</span><span class="sxs-lookup"><span data-stu-id="825ef-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="825ef-112">Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки.</span><span class="sxs-lookup"><span data-stu-id="825ef-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="825ef-113">Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.</span><span class="sxs-lookup"><span data-stu-id="825ef-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="825ef-114">Использование клиента SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="825ef-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="825ef-115">Сослаться на клиентский SignalR JavaScript в `<script>` элемент.</span><span class="sxs-lookup"><span data-stu-id="825ef-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="825ef-116">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="825ef-116">Connect to a hub</span></span>

<span data-ttu-id="825ef-117">Следующий код создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="825ef-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="825ef-118">Имя концентратора регистр не учитывается.</span><span class="sxs-lookup"><span data-stu-id="825ef-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="825ef-119">Независимо от источника соединения</span><span class="sxs-lookup"><span data-stu-id="825ef-119">Cross-origin connections</span></span>

<span data-ttu-id="825ef-120">Как правило браузеры нагрузки подключений из тому же домену, что запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="825ef-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="825ef-121">Тем не менее существуют случаи, когда требуется соединение с другим доменом.</span><span class="sxs-lookup"><span data-stu-id="825ef-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="825ef-122">Для предотвращения чтения конфиденциальных данных с другого сайта, вредоносный сайт [подключения независимо от источника](xref:security/cors) отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="825ef-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="825ef-123">Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="825ef-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="825ef-124">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="825ef-124">Call hub methods from client</span></span>

<span data-ttu-id="825ef-125">Клиенты JavaScript вызывать открытые методы концентраторы через [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) метод [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="825ef-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="825ef-126">`invoke` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="825ef-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="825ef-127">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="825ef-127">The name of the hub method.</span></span> <span data-ttu-id="825ef-128">В следующем примере, является имя метода на концентраторе `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="825ef-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="825ef-129">Все аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="825ef-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="825ef-130">В следующем примере, является имя аргумента `message`.</span><span class="sxs-lookup"><span data-stu-id="825ef-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="825ef-131">В примере кода используется синтаксис функции со стрелкой, которая поддерживается в текущих версиях всех основных браузерах за исключением Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="825ef-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="825ef-132">Если вы используете службу Azure SignalR в *бессерверной режим*, нельзя вызывать методы концентратора клиента.</span><span class="sxs-lookup"><span data-stu-id="825ef-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="825ef-133">Дополнительные сведения см. в разделе [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="825ef-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="825ef-134">`invoke` Метод возвращает JavaScript [обещание](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span><span class="sxs-lookup"><span data-stu-id="825ef-134">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="825ef-135">`Promise` Разрешается с возвращаемым значением (если таковые имеются) при возвращении метода на сервере.</span><span class="sxs-lookup"><span data-stu-id="825ef-135">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="825ef-136">Если на сервере метод вызывает ошибку, `Promise` отклоняется с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="825ef-136">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="825ef-137">Используйте `then` и `catch` методы `Promise` сам для обработки таких случаев (или `await` синтаксиса).</span><span class="sxs-lookup"><span data-stu-id="825ef-137">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="825ef-138">`send` Метод возвращает JavaScript `Promise`.</span><span class="sxs-lookup"><span data-stu-id="825ef-138">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="825ef-139">`Promise` Разрешается, когда сообщение было отправлено на сервер.</span><span class="sxs-lookup"><span data-stu-id="825ef-139">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="825ef-140">Если возникает ошибка отправки сообщений и `Promise` отклоняется с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="825ef-140">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="825ef-141">Используйте `then` и `catch` методы `Promise` сам для обработки таких случаев (или `await` синтаксиса).</span><span class="sxs-lookup"><span data-stu-id="825ef-141">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="825ef-142">С помощью `send` не приходится ждать сервер получил сообщение.</span><span class="sxs-lookup"><span data-stu-id="825ef-142">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="825ef-143">Следовательно он не поддерживается для возвращения данных или ошибки с сервера.</span><span class="sxs-lookup"><span data-stu-id="825ef-143">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="825ef-144">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="825ef-144">Call client methods from hub</span></span>

<span data-ttu-id="825ef-145">Для получения сообщений от концентратора, определение метода с помощью [на](/javascript/api/%40aspnet/signalr/hubconnection#on) метод `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="825ef-145">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="825ef-146">Имя метода клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="825ef-146">The name of the JavaScript client method.</span></span> <span data-ttu-id="825ef-147">В следующем примере, является имя метода `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="825ef-147">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="825ef-148">Аргументы концентратор передает в метод.</span><span class="sxs-lookup"><span data-stu-id="825ef-148">Arguments the hub passes to the method.</span></span> <span data-ttu-id="825ef-149">В следующем примере значение аргумента равно `message`.</span><span class="sxs-lookup"><span data-stu-id="825ef-149">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="825ef-150">Предыдущий код в `connection.on` выполняется, когда серверный код вызывает ее с помощью [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) метод.</span><span class="sxs-lookup"><span data-stu-id="825ef-150">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="825ef-151">SignalR определяют, какой метод клиента для вызова, сопоставляя имя метода и аргументы, определенные в `SendAsync` и `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="825ef-151">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="825ef-152">Рекомендуется, вызовите [запустить](/javascript/api/%40aspnet/signalr/hubconnection#start) метод `HubConnection` после `on`.</span><span class="sxs-lookup"><span data-stu-id="825ef-152">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="825ef-153">Это гарантирует, что зарегистрированных обработчиков до получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="825ef-153">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="825ef-154">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="825ef-154">Error handling and logging</span></span>

<span data-ttu-id="825ef-155">Цепочка `catch` метод в конец `start` метод для обработки ошибок на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="825ef-155">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="825ef-156">Используйте `console.error` для ошибок вывода для консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="825ef-156">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="825ef-157">Настройка трассировки журнала на стороне клиента, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение.</span><span class="sxs-lookup"><span data-stu-id="825ef-157">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="825ef-158">Выводятся сообщения с уровнем журнала и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="825ef-158">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="825ef-159">Ниже перечислены уровни доступных журналов:</span><span class="sxs-lookup"><span data-stu-id="825ef-159">Available log levels are as follows:</span></span>

* <span data-ttu-id="825ef-160">`signalR.LogLevel.Error` &ndash; Сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="825ef-160">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="825ef-161">Журналы `Error` только сообщения.</span><span class="sxs-lookup"><span data-stu-id="825ef-161">Logs `Error` messages only.</span></span>
* <span data-ttu-id="825ef-162">`signalR.LogLevel.Warning` &ndash; Предупреждающие сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="825ef-162">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="825ef-163">Журналы `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="825ef-163">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="825ef-164">`signalR.LogLevel.Information` &ndash; Сообщения о состоянии без ошибок.</span><span class="sxs-lookup"><span data-stu-id="825ef-164">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="825ef-165">Журналы `Information`, `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="825ef-165">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="825ef-166">`signalR.LogLevel.Trace` &ndash; Сообщения трассировки.</span><span class="sxs-lookup"><span data-stu-id="825ef-166">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="825ef-167">В журнал все события, в том числе данных, перемещаемая между центром и клиентом.</span><span class="sxs-lookup"><span data-stu-id="825ef-167">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="825ef-168">Используйте [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) метод [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) настроить уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="825ef-168">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="825ef-169">Сообщения записываются в консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="825ef-169">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="825ef-170">Повторное подключение клиентов</span><span class="sxs-lookup"><span data-stu-id="825ef-170">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="825ef-171">Автоматическое переподключение</span><span class="sxs-lookup"><span data-stu-id="825ef-171">Automatically reconnect</span></span>

<span data-ttu-id="825ef-172">Клиент JavaScript для SignalR можно настроить для автоматического подключения с помощью `withAutomaticReconnect` метод [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="825ef-172">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="825ef-173">Он не будет повторное соединение производится автоматически по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="825ef-173">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="825ef-174">Без параметров `withAutomaticReconnect()` служит для настройки клиента в течение 0, 2, 10 и 30 секунд соответственно, перед попыткой попытками повторного подключения после четырех неудачных попыток.</span><span class="sxs-lookup"><span data-stu-id="825ef-174">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="825ef-175">Прежде чем выполнять любые попытки повторного подключения `HubConnection` будет переход на `HubConnectionState.Reconnecting` состояния и запускать его `onreconnecting` обратные вызовы вместо переход к `Disconnected` состояния и активации его `onclose` обратные вызовы, такие как `HubConnection`настроена без автоматического повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="825ef-175">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="825ef-176">Это дает возможность предупредить пользователей о том, что соединение было потеряно и отключать элементы пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="825ef-176">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="825ef-177">Если клиент успешном повторном подключении в первые четыре попытки, `HubConnection` перейдет к `Connected` состояния и запускать его `onreconnected` обратные вызовы.</span><span class="sxs-lookup"><span data-stu-id="825ef-177">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="825ef-178">Это дает возможность для информирования пользователей, подключение будет восстановлено.</span><span class="sxs-lookup"><span data-stu-id="825ef-178">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="825ef-179">Так как подключение выглядит совершенно новое на сервер новый `connectionId` будет предоставляться `onreconnected` обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="825ef-179">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="825ef-180">`onreconnected` Обратного вызова `connectionId` параметра будет неопределенным, если `HubConnection` был настроен для [пропустить согласования](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="825ef-180">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="825ef-181">`withAutomaticReconnect()` не настроить `HubConnection` попытки при сбоях начальной загрузки, поэтому при сбоях загрузки нужно обрабатывать вручную:</span><span class="sxs-lookup"><span data-stu-id="825ef-181">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="825ef-182">Если клиент не повторного подключения успешно в первые четыре попытки, `HubConnection` будет переход к `Disconnected` состояния и запускать его [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) обратные вызовы.</span><span class="sxs-lookup"><span data-stu-id="825ef-182">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="825ef-183">Это дает возможность для информирования пользователей, соединение постоянно потерян и рекомендует обновить страницу:</span><span class="sxs-lookup"><span data-stu-id="825ef-183">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="825ef-184">Чтобы настроить настраиваемое количество попыток повторного подключения до отключения или повторного подключения этот интервал, `withAutomaticReconnect` принимает массив чисел, представляющий задержку в миллисекундах для ожидания перед началом каждой попытки повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="825ef-184">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="825ef-185">В предыдущем примере настраивается `HubConnection` запустить попытки повторных подключений сразу же после потери связи.</span><span class="sxs-lookup"><span data-stu-id="825ef-185">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="825ef-186">Это справедливо также для настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="825ef-186">This is also true for the default configuration.</span></span>

<span data-ttu-id="825ef-187">В случае с первой попытки повторного подключения со второй попытки повторного подключения будут также применены немедленно вместо ожидания 2 секунды, как в конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="825ef-187">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="825ef-188">В случае со второй попытки повторного подключения третьей попытки повторного подключения начнется через 10 секунд и снова станет как конфигурацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="825ef-188">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="825ef-189">Пользовательское поведение затем еще раз расходится со всеми поведение по умолчанию путем остановки после сбоя вместо того, один попытки третий повторное соединение более повторного подключения попытки еще на 30 секунд, как в конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="825ef-189">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="825ef-190">Если вы хотите еще лучше контролировать время и число автоматическое переподключение попыток, `withAutomaticReconnect` принимает объект, реализующий интерфейс `IReconnectPolicy` интерфейс, который содержит один метод с именем `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="825ef-190">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="825ef-191">`nextRetryDelayInMilliseconds` принимает два аргумента: `previousRetryCount` и `elapsedMilliseconds`, которые являются оба числа.</span><span class="sxs-lookup"><span data-stu-id="825ef-191">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="825ef-192">Перед первой попытки повторного подключения оба `previousRetryCount` и `elapsedMilliseconds` будут равны нулю.</span><span class="sxs-lookup"><span data-stu-id="825ef-192">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="825ef-193">После каждой попытки повтора неудачных `previousRetryCount` увеличивается на единицу и `elapsedMilliseconds` будут обновлены в соответствии с время, затраченное на повторное подключение в данный момент в миллисекундах.</span><span class="sxs-lookup"><span data-stu-id="825ef-193">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="825ef-194">`nextRetryDelayInMilliseconds` должен возвращать либо число, представляющее количество миллисекунд ожидания перед следующей попыткой повторного подключения или `null` Если `HubConnection` должна прекратиться, повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="825ef-194">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        }
    })
    .build();
```

<span data-ttu-id="825ef-195">Кроме того, можно написать код, который позволит приложению повторно подключиться клиент вручную, как показано в [вручную переподключить](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="825ef-195">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="825ef-196">Вручную переподключить</span><span class="sxs-lookup"><span data-stu-id="825ef-196">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="825ef-197">До версии 3.0 клиент JavaScript для SignalR не повторное соединение производится автоматически.</span><span class="sxs-lookup"><span data-stu-id="825ef-197">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="825ef-198">Необходимо написать код, который позволит приложению повторно подключиться клиент вручную.</span><span class="sxs-lookup"><span data-stu-id="825ef-198">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="825ef-199">Следующий код демонстрирует подход обычно ручного повторного подключения:</span><span class="sxs-lookup"><span data-stu-id="825ef-199">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="825ef-200">Функции (в этом случае `start` функции) создается при запуске подключения.</span><span class="sxs-lookup"><span data-stu-id="825ef-200">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="825ef-201">Вызовите `start` функции в соединении `onclose` обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="825ef-201">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="825ef-202">Реальную реализацию будут использовать стратегию экспоненциальной отсрочки или повторите указанное число раз, прежде чем прекратить.</span><span class="sxs-lookup"><span data-stu-id="825ef-202">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="825ef-203">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="825ef-203">Additional resources</span></span>

* [<span data-ttu-id="825ef-204">Справочник по API JavaScript</span><span class="sxs-lookup"><span data-stu-id="825ef-204">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="825ef-205">Учебник по JavaScript</span><span class="sxs-lookup"><span data-stu-id="825ef-205">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="825ef-206">Руководство по WebPack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="825ef-206">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="825ef-207">Центры</span><span class="sxs-lookup"><span data-stu-id="825ef-207">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="825ef-208">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="825ef-208">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="825ef-209">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="825ef-209">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="825ef-210">Запросов о происхождении (CORS)</span><span class="sxs-lookup"><span data-stu-id="825ef-210">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="825ef-211">SignalR бессерверной документация по службе Azure</span><span class="sxs-lookup"><span data-stu-id="825ef-211">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
