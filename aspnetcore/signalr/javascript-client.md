---
title: Клиент ASP.NET Core SignalR JavaScript
author: tdykstra
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 247ccd40412cdb41f38edccbe96d4832751f12cf
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861996"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="54823-103">Клиент ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="54823-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="54823-104">Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="54823-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="54823-105">Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="54823-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="54823-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="54823-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="54823-107">Установка пакета клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="54823-107">Install the SignalR client package</span></span>

<span data-ttu-id="54823-108">Клиентская библиотека SignalR JavaScript предоставляется в виде [npm](https://www.npmjs.com/) пакета.</span><span class="sxs-lookup"><span data-stu-id="54823-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="54823-109">Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="54823-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="54823-110">Для Visual Studio Code, выполните команду из **интегрированный терминал**.</span><span class="sxs-lookup"><span data-stu-id="54823-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="54823-111">npm устанавливает содержимое пакета в *node_modules\\@aspnet\signalr\dist\browser* папки.</span><span class="sxs-lookup"><span data-stu-id="54823-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="54823-112">Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки.</span><span class="sxs-lookup"><span data-stu-id="54823-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="54823-113">Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.</span><span class="sxs-lookup"><span data-stu-id="54823-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="54823-114">Использование клиента SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="54823-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="54823-115">Сослаться на клиентский SignalR JavaScript в `<script>` элемент.</span><span class="sxs-lookup"><span data-stu-id="54823-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="54823-116">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="54823-116">Connect to a hub</span></span>

<span data-ttu-id="54823-117">Следующий код создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="54823-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="54823-118">Имя концентратора регистр не учитывается.</span><span class="sxs-lookup"><span data-stu-id="54823-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="54823-119">Независимо от источника соединения</span><span class="sxs-lookup"><span data-stu-id="54823-119">Cross-origin connections</span></span>

<span data-ttu-id="54823-120">Как правило браузеры нагрузки подключений из тому же домену, что запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="54823-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="54823-121">Тем не менее существуют случаи, когда требуется соединение с другим доменом.</span><span class="sxs-lookup"><span data-stu-id="54823-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="54823-122">Для предотвращения чтения конфиденциальных данных с другого сайта, вредоносный сайт [подключения независимо от источника](xref:security/cors) отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54823-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="54823-123">Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="54823-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="54823-124">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="54823-124">Call hub methods from client</span></span>

<span data-ttu-id="54823-125">Клиенты JavaScript вызывать открытые методы концентраторы через [вызова](/javascript/api/%40aspnet/signalr/hubconnection#invoke) метод [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="54823-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="54823-126">`invoke` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="54823-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="54823-127">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="54823-127">The name of the hub method.</span></span> <span data-ttu-id="54823-128">В следующем примере, является имя метода на концентраторе `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="54823-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="54823-129">Все аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="54823-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="54823-130">В следующем примере, является имя аргумента `message`.</span><span class="sxs-lookup"><span data-stu-id="54823-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="54823-131">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="54823-131">Call client methods from hub</span></span>

<span data-ttu-id="54823-132">Для получения сообщений от концентратора, определение метода с помощью [на](/javascript/api/%40aspnet/signalr/hubconnection#on) метод `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="54823-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="54823-133">Имя метода клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54823-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="54823-134">В следующем примере, является имя метода `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="54823-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="54823-135">Аргументы концентратор передает в метод.</span><span class="sxs-lookup"><span data-stu-id="54823-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="54823-136">В следующем примере значение аргумента равно `message`.</span><span class="sxs-lookup"><span data-stu-id="54823-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="54823-137">Предыдущий код в `connection.on` выполняется, когда серверный код вызывает ее с помощью [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) метод.</span><span class="sxs-lookup"><span data-stu-id="54823-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="54823-138">SignalR определяют, какой метод клиента для вызова, сопоставляя имя метода и аргументы, определенные в `SendAsync` и `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="54823-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="54823-139">Рекомендуется, вызовите [запустить](/javascript/api/%40aspnet/signalr/hubconnection#start) метод `HubConnection` после `on`.</span><span class="sxs-lookup"><span data-stu-id="54823-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="54823-140">Это гарантирует, что зарегистрированных обработчиков до получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="54823-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="54823-141">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="54823-141">Error handling and logging</span></span>

<span data-ttu-id="54823-142">Цепочка `catch` метод в конец `start` метод для обработки ошибок на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="54823-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="54823-143">Используйте `console.error` для ошибок вывода для консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="54823-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="54823-144">Настройка трассировки журнала на стороне клиента, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение.</span><span class="sxs-lookup"><span data-stu-id="54823-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="54823-145">Выводятся сообщения с уровнем журнала и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="54823-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="54823-146">Ниже перечислены уровни доступных журналов:</span><span class="sxs-lookup"><span data-stu-id="54823-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="54823-147">`signalR.LogLevel.Error` &ndash; Сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="54823-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="54823-148">Журналы `Error` только сообщения.</span><span class="sxs-lookup"><span data-stu-id="54823-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="54823-149">`signalR.LogLevel.Warning` &ndash; Предупреждающие сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="54823-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="54823-150">Журналы `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="54823-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="54823-151">`signalR.LogLevel.Information` &ndash; Сообщения о состоянии без ошибок.</span><span class="sxs-lookup"><span data-stu-id="54823-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="54823-152">Журналы `Information`, `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="54823-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="54823-153">`signalR.LogLevel.Trace` &ndash; Сообщения трассировки.</span><span class="sxs-lookup"><span data-stu-id="54823-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="54823-154">В журнал все события, в том числе данных, перемещаемая между центром и клиентом.</span><span class="sxs-lookup"><span data-stu-id="54823-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="54823-155">Используйте [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) метод [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) настроить уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="54823-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="54823-156">Сообщения записываются в консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="54823-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="54823-157">Повторное подключение клиентов</span><span class="sxs-lookup"><span data-stu-id="54823-157">Reconnect clients</span></span>

<span data-ttu-id="54823-158">Клиент JavaScript для SignalR не повторное соединение производится автоматически.</span><span class="sxs-lookup"><span data-stu-id="54823-158">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="54823-159">Необходимо написать код, который позволит приложению повторно подключиться клиент вручную.</span><span class="sxs-lookup"><span data-stu-id="54823-159">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="54823-160">Следующий код демонстрирует подход обычно повторного подключения:</span><span class="sxs-lookup"><span data-stu-id="54823-160">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="54823-161">Функции (в этом случае `start` функции) создается при запуске подключения.</span><span class="sxs-lookup"><span data-stu-id="54823-161">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="54823-162">Вызовите `start` функции в соединении `onclose` обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="54823-162">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=30-42)]

<span data-ttu-id="54823-163">Реальную реализацию будут использовать стратегию экспоненциальной отсрочки или повторите указанное число раз, прежде чем прекратить.</span><span class="sxs-lookup"><span data-stu-id="54823-163">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="54823-164">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="54823-164">Additional resources</span></span>

* [<span data-ttu-id="54823-165">Справочник по API JavaScript</span><span class="sxs-lookup"><span data-stu-id="54823-165">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="54823-166">Учебник по JavaScript</span><span class="sxs-lookup"><span data-stu-id="54823-166">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="54823-167">Руководство по WebPack и TypeScript</span><span class="sxs-lookup"><span data-stu-id="54823-167">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="54823-168">Центры</span><span class="sxs-lookup"><span data-stu-id="54823-168">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="54823-169">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="54823-169">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="54823-170">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="54823-170">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="54823-171">Запросов о происхождении (CORS)</span><span class="sxs-lookup"><span data-stu-id="54823-171">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
