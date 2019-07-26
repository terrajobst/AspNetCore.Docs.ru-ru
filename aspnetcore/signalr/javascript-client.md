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
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="cbe01-103">Клиент JavaScript ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cbe01-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="cbe01-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="cbe01-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="cbe01-105">Клиентская библиотека JavaScript ASP.NET Core SignalR позволяет разработчикам вызывать код концентратора на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="cbe01-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="cbe01-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cbe01-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="cbe01-107">Установка клиентского пакета SignalR</span><span class="sxs-lookup"><span data-stu-id="cbe01-107">Install the SignalR client package</span></span>

<span data-ttu-id="cbe01-108">Клиентская библиотека JavaScript SignalR поставляется в виде пакета [NPM](https://www.npmjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="cbe01-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="cbe01-109">Если вы используете Visual Studio, запустите `npm install` из **консоли диспетчера пакетов** , находясь в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="cbe01-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="cbe01-110">Для Visual Studio Code выполните команду из **интегрированного терминала**.</span><span class="sxs-lookup"><span data-stu-id="cbe01-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="cbe01-111">npm устанавливает содержимое пакета в *node_modules\\@microsoft\signalr\dist\browser* папки.</span><span class="sxs-lookup"><span data-stu-id="cbe01-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="cbe01-112">Создайте новую папку с именем *SignalR* в папке *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="cbe01-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="cbe01-113">Скопируйте файл *SignalR. js* в папку *ввврут\либ\сигналр*</span><span class="sxs-lookup"><span data-stu-id="cbe01-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="cbe01-114">npm устанавливает содержимое пакета в *node_modules\\@aspnet\signalr\dist\browser* папки.</span><span class="sxs-lookup"><span data-stu-id="cbe01-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="cbe01-115">Создайте новую папку с именем *SignalR* в папке *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="cbe01-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="cbe01-116">Скопируйте файл *SignalR. js* в папку *ввврут\либ\сигналр*</span><span class="sxs-lookup"><span data-stu-id="cbe01-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="cbe01-117">Использовать клиент JavaScript SignalR</span><span class="sxs-lookup"><span data-stu-id="cbe01-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="cbe01-118">Ссылка на клиент JavaScript SignalR в `<script>` элементе.</span><span class="sxs-lookup"><span data-stu-id="cbe01-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="cbe01-119">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="cbe01-119">Connect to a hub</span></span>

<span data-ttu-id="cbe01-120">Следующий код создает и запускает соединение.</span><span class="sxs-lookup"><span data-stu-id="cbe01-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="cbe01-121">Имя центра не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="cbe01-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="cbe01-122">Подключения между источниками</span><span class="sxs-lookup"><span data-stu-id="cbe01-122">Cross-origin connections</span></span>

<span data-ttu-id="cbe01-123">Как правило, Браузеры загружают соединения из того же домена, что и запрошенная страница.</span><span class="sxs-lookup"><span data-stu-id="cbe01-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="cbe01-124">Однако бывают случаи, когда требуется подключение к другому домену.</span><span class="sxs-lookup"><span data-stu-id="cbe01-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="cbe01-125">Чтобы предотвратить чтение конфиденциальных данных с другого сайта вредоносным сайтом, [межисточниковые подключения](xref:security/cors) отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cbe01-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="cbe01-126">Чтобы разрешить запрос между источниками, включите его в `Startup` классе.</span><span class="sxs-lookup"><span data-stu-id="cbe01-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="cbe01-127">Методы концентратора вызовов от клиента</span><span class="sxs-lookup"><span data-stu-id="cbe01-127">Call hub methods from client</span></span>

<span data-ttu-id="cbe01-128">Клиенты JavaScript вызывают открытые методы на концентраторах с помощью метода [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) [хубконнектион](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="cbe01-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="cbe01-129">`invoke` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="cbe01-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="cbe01-130">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="cbe01-130">The name of the hub method.</span></span> <span data-ttu-id="cbe01-131">В следующем примере имя метода в концентраторе — `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="cbe01-132">Любые аргументы, определенные в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="cbe01-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="cbe01-133">В следующем примере имя аргумента — `message`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="cbe01-134">В примере кода используется синтаксис функции стрелок, который поддерживается в текущих версиях всех основных браузеров, кроме Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="cbe01-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="cbe01-135">Если вы используете службу Azure SignalR в бессерверном *режиме*, вы не можете вызывать методы концентратора из клиента.</span><span class="sxs-lookup"><span data-stu-id="cbe01-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="cbe01-136">Дополнительные сведения см. в [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="cbe01-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="cbe01-137">Метод возвращает обещание JavaScript. [](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) `invoke`</span><span class="sxs-lookup"><span data-stu-id="cbe01-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="cbe01-138">`Promise` Разрешение разрешается с возвращаемым значением (если таковое имеется), когда метод на сервере возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="cbe01-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="cbe01-139">Если метод на сервере вызывает ошибку, объект `Promise` отклоняется с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="cbe01-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="cbe01-140">`catch` Используйтеметоды`Promise` идля`await` самого себя, чтобы обрабатывали такие случаи (или синтаксис). `then`</span><span class="sxs-lookup"><span data-stu-id="cbe01-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="cbe01-141">Метод возвращает код JavaScript `Promise`. `send`</span><span class="sxs-lookup"><span data-stu-id="cbe01-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="cbe01-142">`Promise` Разрешение разрешается, когда сообщение было отправлено на сервер.</span><span class="sxs-lookup"><span data-stu-id="cbe01-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="cbe01-143">Если при отправке сообщения произошла ошибка, объект `Promise` отклоняется с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="cbe01-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="cbe01-144">`catch` Используйтеметоды`Promise` идля`await` самого себя, чтобы обрабатывали такие случаи (или синтаксис). `then`</span><span class="sxs-lookup"><span data-stu-id="cbe01-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="cbe01-145">Использование `send` не ждет, пока сервер не получит сообщение.</span><span class="sxs-lookup"><span data-stu-id="cbe01-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="cbe01-146">Следовательно, невозможно вернуть данные или ошибки с сервера.</span><span class="sxs-lookup"><span data-stu-id="cbe01-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="cbe01-147">Вызов методов клиента из концентратора</span><span class="sxs-lookup"><span data-stu-id="cbe01-147">Call client methods from hub</span></span>

<span data-ttu-id="cbe01-148">Чтобы получить сообщения от концентратора, определите метод с помощью метода [On](/javascript/api/%40aspnet/signalr/hubconnection#on) класса `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="cbe01-149">Имя клиентского метода JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cbe01-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="cbe01-150">В следующем примере имя метода — `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="cbe01-151">Аргументы, которые центр передает в метод.</span><span class="sxs-lookup"><span data-stu-id="cbe01-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="cbe01-152">В следующем примере аргумент имеет `message`значение.</span><span class="sxs-lookup"><span data-stu-id="cbe01-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="cbe01-153">Приведенный выше код `connection.on` выполняется, когда код на стороне сервера вызывает его с помощью метода [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .</span><span class="sxs-lookup"><span data-stu-id="cbe01-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="cbe01-154">SignalR определяет, какой клиентский метод следует вызывать, сопоставляя имя и аргументы метода, определенные `SendAsync` в `connection.on`и.</span><span class="sxs-lookup"><span data-stu-id="cbe01-154">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="cbe01-155">Рекомендуется вызвать метод [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) для `HubConnection` after `on`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="cbe01-156">Это гарантирует, что обработчики будут зарегистрированы до получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="cbe01-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="cbe01-157">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="cbe01-157">Error handling and logging</span></span>

<span data-ttu-id="cbe01-158">`start` Привязать `catch` метод к концу метода для управления ошибками на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="cbe01-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="cbe01-159">Используется `console.error` для вывода ошибок в консоль браузера.</span><span class="sxs-lookup"><span data-stu-id="cbe01-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="cbe01-160">Настройте трассировку журнала на стороне клиента, передав средства ведения журнала и тип события в журнал, когда соединение установлено.</span><span class="sxs-lookup"><span data-stu-id="cbe01-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="cbe01-161">Сообщения записываются с указанным уровнем ведения журнала и выше.</span><span class="sxs-lookup"><span data-stu-id="cbe01-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="cbe01-162">Доступны следующие уровни ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="cbe01-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="cbe01-163">`signalR.LogLevel.Error`&ndash; Сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="cbe01-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="cbe01-164">Записывает `Error` только сообщения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="cbe01-165">`signalR.LogLevel.Warning`&ndash; Предупреждающие сообщения о возможных ошибках.</span><span class="sxs-lookup"><span data-stu-id="cbe01-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="cbe01-166">Журналы `Warning` и`Error` сообщения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="cbe01-167">`signalR.LogLevel.Information`&ndash; Сообщения о состоянии без ошибок.</span><span class="sxs-lookup"><span data-stu-id="cbe01-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="cbe01-168">Журналы `Information`, `Warning` и`Error` сообщения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="cbe01-169">`signalR.LogLevel.Trace`&ndash; Сообщения трассировки.</span><span class="sxs-lookup"><span data-stu-id="cbe01-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="cbe01-170">Заносит в журнал все данные, включая транспорт данных между концентратором и клиентом.</span><span class="sxs-lookup"><span data-stu-id="cbe01-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="cbe01-171">Чтобы настроить уровень ведения журнала, используйте метод [конфигурелоггинг](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) в [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="cbe01-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="cbe01-172">Сообщения записываются в консоль браузера.</span><span class="sxs-lookup"><span data-stu-id="cbe01-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="cbe01-173">Повторное подключение клиентов</span><span class="sxs-lookup"><span data-stu-id="cbe01-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="cbe01-174">Автоматическое повторное подключение</span><span class="sxs-lookup"><span data-stu-id="cbe01-174">Automatically reconnect</span></span>

<span data-ttu-id="cbe01-175">Клиент JavaScript для SignalR можно настроить для автоматического повторного подключения с помощью `withAutomaticReconnect` метода в [хубконнектионбуилдер](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="cbe01-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="cbe01-176">По умолчанию оно не будет автоматически восстановлено.</span><span class="sxs-lookup"><span data-stu-id="cbe01-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="cbe01-177">Без каких бы то `withAutomaticReconnect()` ни было параметров, настраивает клиент на ожидание 0, 2, 10 и 30 секунд соответственно, прежде чем пытаться выполнить каждую попытку повторного подключения, останавливая после четырех неудачных попыток.</span><span class="sxs-lookup"><span data-stu-id="cbe01-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="cbe01-178">Перед запуском всех попыток `HubConnection` повторного подключения компонент переходит `HubConnectionState.Reconnecting` в состояние и срабатывает `onreconnecting` на `Disconnected` обратные вызовы вместо перехода в состояние и запуска своих `onclose` обратных вызовов, таких как `HubConnection`без настроенного автоматического повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="cbe01-179">Это дает возможность предупредить пользователей о потере подключения и отключении элементов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="cbe01-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="cbe01-180">Если клиент успешно повторно подключится в течение первых четырех попыток, `HubConnection` компонент вернется `Connected` к состоянию и порождает его `onreconnected` обратные вызовы.</span><span class="sxs-lookup"><span data-stu-id="cbe01-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="cbe01-181">Это дает возможность информировать пользователей о повторном установлении соединения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="cbe01-182">Так как соединение выглядит совершенно не так, как и сервер, `connectionId` `onreconnected` для обратного вызова будет предоставлен новый объект.</span><span class="sxs-lookup"><span data-stu-id="cbe01-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="cbe01-183">Параметр обратного вызова будет неопределенным `HubConnection` , если был настроен на [пропуск согласования](xref:signalr/configuration#configure-client-options). `onreconnected` `connectionId`</span><span class="sxs-lookup"><span data-stu-id="cbe01-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="cbe01-184">`withAutomaticReconnect()`не настраивается `HubConnection` для повтора начальных сбоев запуска, поэтому ошибки запуска должны быть обработаны вручную:</span><span class="sxs-lookup"><span data-stu-id="cbe01-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="cbe01-185">Если клиент не `HubConnection` будет успешно подключаться в течение первых четырех попыток, компонент перейдет `Disconnected` в состояние и порождает его обратные вызовы OnClose. [](/javascript/api/%40aspnet/signalr/hubconnection#onclose)</span><span class="sxs-lookup"><span data-stu-id="cbe01-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="cbe01-186">Это дает возможность информировать пользователей о том, что подключение было безвозвратно утрачено, и рекомендует обновить страницу:</span><span class="sxs-lookup"><span data-stu-id="cbe01-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="cbe01-187">Чтобы настроить настраиваемое число попыток повторного подключения перед отключением или изменением времени повторного подключения, принимает `withAutomaticReconnect` массив чисел, представляющих задержку в миллисекундах, прежде чем начинать каждую попытку повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="cbe01-188">В предыдущем примере настраивается `HubConnection` для запуска попытки повторного подключения сразу после потери соединения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="cbe01-189">Это также справедливо для конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cbe01-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="cbe01-190">Если первая попытка повторного подключения завершается неудачно, вторая попытка повторного подключения также начнется немедленно, а не подождать 2 секунды, как в конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cbe01-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="cbe01-191">Если вторая попытка повторного подключения завершается неудачно, третья попытка повторного подключения начнется через 10 секунд, что опять же, как и конфигурация по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cbe01-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="cbe01-192">Затем пользовательское поведение снова рассходится от поведения по умолчанию, останавливаясь после третьей попытки повторного подключения, вместо того, чтобы предпринимать попытку повторного подключения еще через 30 секунд, как в конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cbe01-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="cbe01-193">Если требуется даже больший контроль над временем и числом попыток автоматического повторного подключения, `withAutomaticReconnect` принимает объект, `IRetryPolicy` реализующий интерфейс, который имеет единственный метод с именем `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="cbe01-194">`nextRetryDelayInMilliseconds`принимает один аргумент с типом `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="cbe01-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="cbe01-195">`elapsedMilliseconds` Имееттри`Error` свойства `previousRetryCount`: `retryReason` , и, а — ,`number` и соответственно. `number` `RetryContext`</span><span class="sxs-lookup"><span data-stu-id="cbe01-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="cbe01-196">Перед первой попыткой повторного подключения обе `previousRetryCount` и `elapsedMilliseconds` будут равны нулю, и `retryReason` будет являться ошибкой, вызвавшей потерю соединения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="cbe01-197">После каждой неудачной попытки `previousRetryCount` повтора значение будет увеличено на единицу, `elapsedMilliseconds` будет обновлено с учетом времени, затраченного на повторное подключение к этому моменту в `retryReason` миллисекундах, и будет являться ошибкой, вызвавшей Последнее повторение попытки повторного подключения. Cчетчик.</span><span class="sxs-lookup"><span data-stu-id="cbe01-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="cbe01-198">`nextRetryDelayInMilliseconds`должен возвращать число, представляющее число миллисекунд ожидания перед следующей попыткой повторного подключения, или `null` значение, `HubConnection` если должно быть прервано повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="cbe01-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="cbe01-199">Кроме того, можно написать код, который будет повторно подключаться к клиенту вручную, как показано в подсоединении [вручную](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="cbe01-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="cbe01-200">Повторное подключение вручную</span><span class="sxs-lookup"><span data-stu-id="cbe01-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="cbe01-201">До 3,0 клиент JavaScript для SignalR не переключается автоматически.</span><span class="sxs-lookup"><span data-stu-id="cbe01-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="cbe01-202">Необходимо написать код, который будет повторно подключаться к клиенту вручную.</span><span class="sxs-lookup"><span data-stu-id="cbe01-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="cbe01-203">В следующем коде показан типичный способ повторного подключения вручную:</span><span class="sxs-lookup"><span data-stu-id="cbe01-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="cbe01-204">Для запуска подключения создается функция (в данном `start` случае это функция).</span><span class="sxs-lookup"><span data-stu-id="cbe01-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="cbe01-205">`onclose` Вызовите `start` функцию в обработчике событий соединения.</span><span class="sxs-lookup"><span data-stu-id="cbe01-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="cbe01-206">В реальной реализации будет использоваться экспоненциальная отсрочка или повторная попытка заданное число раз.</span><span class="sxs-lookup"><span data-stu-id="cbe01-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cbe01-207">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cbe01-207">Additional resources</span></span>

* [<span data-ttu-id="cbe01-208">Справочник по API JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbe01-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="cbe01-209">Учебник по JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbe01-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="cbe01-210">Руководство по пакету и TypeScript</span><span class="sxs-lookup"><span data-stu-id="cbe01-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="cbe01-211">Центры</span><span class="sxs-lookup"><span data-stu-id="cbe01-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cbe01-212">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="cbe01-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="cbe01-213">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="cbe01-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="cbe01-214">Запросы между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="cbe01-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="cbe01-215">Документация, посвященная серверу службы Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="cbe01-215">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
