---
title: Клиент ASP.NET Core SignalR JavaScript
author: rachelappel
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938048"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="f4c85-103">Клиент ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="f4c85-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="f4c85-104">Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="f4c85-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f4c85-105">Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="f4c85-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="f4c85-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4c85-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="f4c85-107">Установка пакета клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="f4c85-107">Install the SignalR client package</span></span>

<span data-ttu-id="f4c85-108">Клиентская библиотека SignalR JavaScript предоставляется в виде [npm](https://www.npmjs.com/) пакета.</span><span class="sxs-lookup"><span data-stu-id="f4c85-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="f4c85-109">Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="f4c85-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="f4c85-110">Для Visual Studio Code, выполните команду из **интегрированный терминал**.</span><span class="sxs-lookup"><span data-stu-id="f4c85-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="f4c85-111">Npm устанавливает содержимое пакета в *node_modules\\ @aspnet\signalr\dist\browser*  папки.</span><span class="sxs-lookup"><span data-stu-id="f4c85-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="f4c85-112">Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки.</span><span class="sxs-lookup"><span data-stu-id="f4c85-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="f4c85-113">Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.</span><span class="sxs-lookup"><span data-stu-id="f4c85-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="f4c85-114">Использование клиента SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="f4c85-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="f4c85-115">Сослаться на клиентский SignalR JavaScript в `<script>` элемент.</span><span class="sxs-lookup"><span data-stu-id="f4c85-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="f4c85-116">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="f4c85-116">Connect to a hub</span></span>

<span data-ttu-id="f4c85-117">Следующий код создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="f4c85-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="f4c85-118">Имя концентратора регистр не учитывается.</span><span class="sxs-lookup"><span data-stu-id="f4c85-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="f4c85-119">Независимо от источника соединения</span><span class="sxs-lookup"><span data-stu-id="f4c85-119">Cross-origin connections</span></span>

<span data-ttu-id="f4c85-120">Как правило браузеры нагрузки подключений из тому же домену, что запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="f4c85-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="f4c85-121">Тем не менее существуют случаи, когда требуется соединение с другим доменом.</span><span class="sxs-lookup"><span data-stu-id="f4c85-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="f4c85-122">Для предотвращения чтения конфиденциальных данных с другого сайта, вредоносный сайт [подключения независимо от источника](xref:security/cors) отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f4c85-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="f4c85-123">Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="f4c85-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f4c85-124">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="f4c85-124">Call hub methods from client</span></span>

<span data-ttu-id="f4c85-125">Клиенты JavaScript вызывать открытые методы в концентраторах, с помощью `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="f4c85-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="f4c85-126">`invoke` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="f4c85-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="f4c85-127">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="f4c85-127">The name of the hub method.</span></span> <span data-ttu-id="f4c85-128">В следующем примере имеет имя центра `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="f4c85-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="f4c85-129">Все аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="f4c85-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="f4c85-130">В следующем примере, является имя аргумента `message`.</span><span class="sxs-lookup"><span data-stu-id="f4c85-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f4c85-131">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="f4c85-131">Call client methods from hub</span></span>

<span data-ttu-id="f4c85-132">Для получения сообщений от концентратора, определение метода с помощью `connection.on` метод.</span><span class="sxs-lookup"><span data-stu-id="f4c85-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="f4c85-133">Имя метода клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f4c85-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="f4c85-134">В следующем примере, является имя метода `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="f4c85-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="f4c85-135">Аргументы концентратор передает в метод.</span><span class="sxs-lookup"><span data-stu-id="f4c85-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="f4c85-136">В следующем примере значение аргумента равно `message`.</span><span class="sxs-lookup"><span data-stu-id="f4c85-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="f4c85-137">Предыдущий код в `connection.on` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="f4c85-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="f4c85-138">SignalR определяют, какой метод клиента для вызова, сопоставляя имя метода и аргументы, определенные в `SendAsync` и `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="f4c85-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="f4c85-139">Рекомендуется, вызовите `connection.start` после `connection.on` обработчики зарегистрированные до получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="f4c85-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="f4c85-140">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="f4c85-140">Error handling and logging</span></span>

<span data-ttu-id="f4c85-141">Цепочка `catch` метод в конец `connection.start` метод для обработки ошибок на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f4c85-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="f4c85-142">Используйте `console.error` для ошибок вывода для консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="f4c85-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="f4c85-143">Настройка трассировки журнала на стороне клиента, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение.</span><span class="sxs-lookup"><span data-stu-id="f4c85-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="f4c85-144">Выводятся сообщения с уровнем журнала и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="f4c85-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="f4c85-145">Ниже перечислены уровни доступных журналов:</span><span class="sxs-lookup"><span data-stu-id="f4c85-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="f4c85-146">`signalR.LogLevel.Error` : Сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="f4c85-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="f4c85-147">Журналы `Error` только сообщения.</span><span class="sxs-lookup"><span data-stu-id="f4c85-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="f4c85-148">`signalR.LogLevel.Warning` : Предупреждение сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="f4c85-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="f4c85-149">Журналы `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="f4c85-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="f4c85-150">`signalR.LogLevel.Information` : Сообщения о состоянии без ошибок.</span><span class="sxs-lookup"><span data-stu-id="f4c85-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="f4c85-151">Журналы `Information`, `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="f4c85-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="f4c85-152">`signalR.LogLevel.Trace` : Сообщения трассировки.</span><span class="sxs-lookup"><span data-stu-id="f4c85-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="f4c85-153">В журнал все события, в том числе данных, перемещаемая между центром и клиентом.</span><span class="sxs-lookup"><span data-stu-id="f4c85-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="f4c85-154">Используйте `configureLogging` метод `HubConnectionBuilder` настроить уровень ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="f4c85-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="f4c85-155">Сообщения записываются в консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="f4c85-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="f4c85-156">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f4c85-156">Related resources</span></span>

* [<span data-ttu-id="f4c85-157">Центры</span><span class="sxs-lookup"><span data-stu-id="f4c85-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f4c85-158">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="f4c85-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f4c85-159">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="f4c85-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="f4c85-160">Включение запросов о происхождении (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4c85-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
