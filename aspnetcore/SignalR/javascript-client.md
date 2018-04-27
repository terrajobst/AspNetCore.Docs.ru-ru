---
title: ASP.NET SignalR JavaScript ядра клиента
author: rachelappel
description: Обзор клиента ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: cca1a523bd536f4365e54c196f6c9024779d5d76
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="4b18a-103">ASP.NET SignalR JavaScript ядра клиента</span><span class="sxs-lookup"><span data-stu-id="4b18a-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="4b18a-104">Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="4b18a-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="4b18a-105">Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="4b18a-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="4b18a-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4b18a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="4b18a-107">Установить пакет клиента SignalR</span><span class="sxs-lookup"><span data-stu-id="4b18a-107">Install the SignalR client package</span></span>

<span data-ttu-id="4b18a-108">Клиентская библиотека SignalR JavaScript доставляется в виде [npm](https://www.npmjs.com/) пакета.</span><span class="sxs-lookup"><span data-stu-id="4b18a-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="4b18a-109">Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке.</span><span class="sxs-lookup"><span data-stu-id="4b18a-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="4b18a-110">Для кода Visual Studio, выполните команду из **интеграции терминалов**.</span><span class="sxs-lookup"><span data-stu-id="4b18a-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="4b18a-111">Содержимое пакета в устанавливает Npm *node_modules\\ @aspnet\signalr\dist\browser*  папки.</span><span class="sxs-lookup"><span data-stu-id="4b18a-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="4b18a-112">Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки.</span><span class="sxs-lookup"><span data-stu-id="4b18a-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="4b18a-113">Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.</span><span class="sxs-lookup"><span data-stu-id="4b18a-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="4b18a-114">Использование клиента SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b18a-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="4b18a-115">Ссылки в клиента SignalR JavaScript `<script>` элемента.</span><span class="sxs-lookup"><span data-stu-id="4b18a-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="4b18a-116">Подключения к концентратору</span><span class="sxs-lookup"><span data-stu-id="4b18a-116">Connect to a hub</span></span>

<span data-ttu-id="4b18a-117">Следующий код создает и запускает подключение.</span><span class="sxs-lookup"><span data-stu-id="4b18a-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="4b18a-118">Имя концентратора без учета регистра.</span><span class="sxs-lookup"><span data-stu-id="4b18a-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=1-2,18)]

### <a name="cross-origin-connections"></a><span data-ttu-id="4b18a-119">Независимо от источника подключения</span><span class="sxs-lookup"><span data-stu-id="4b18a-119">Cross-origin connections</span></span>

<span data-ttu-id="4b18a-120">Как правило браузеры загрузить подключений из того же домена, что запрошенной страницы.</span><span class="sxs-lookup"><span data-stu-id="4b18a-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="4b18a-121">Однако существуют случаи, когда требуется соединение с другим доменом.</span><span class="sxs-lookup"><span data-stu-id="4b18a-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="4b18a-122">Для предотвращения чтения конфиденциальных данных с другого сайта вредоносный сайт [подключения независимо от источника](xref:security/cors) по умолчанию отключены.</span><span class="sxs-lookup"><span data-stu-id="4b18a-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="4b18a-123">Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="4b18a-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="4b18a-124">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="4b18a-124">Call hub methods from client</span></span>

<span data-ttu-id="4b18a-125">JavaScript клиенты вызывать открытые методы концентраторы, с помощью `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="4b18a-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="4b18a-126">`invoke` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="4b18a-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="4b18a-127">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="4b18a-127">The name of the hub method.</span></span> <span data-ttu-id="4b18a-128">В следующем примере имеет имя концентратора `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="4b18a-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="4b18a-129">Все аргументы, заданные в метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="4b18a-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="4b18a-130">В следующем примере имя аргумента является `message`.</span><span class="sxs-lookup"><span data-stu-id="4b18a-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=14)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="4b18a-131">Вызовите методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="4b18a-131">Call client methods from hub</span></span>

<span data-ttu-id="4b18a-132">Чтобы получать сообщения от концентратора, определение метода с помощью `connection.on` метод.</span><span class="sxs-lookup"><span data-stu-id="4b18a-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="4b18a-133">Имя метода клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4b18a-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="4b18a-134">В следующем примере имя метода является `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="4b18a-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="4b18a-135">Метод передает аргументы концентратора.</span><span class="sxs-lookup"><span data-stu-id="4b18a-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="4b18a-136">В следующем примере значение аргумента равно `message`.</span><span class="sxs-lookup"><span data-stu-id="4b18a-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=4-9)]

<span data-ttu-id="4b18a-137">Приведенный выше код в `connection.on` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="4b18a-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="4b18a-138">SignalR определяют, какой метод клиента для вызова путем сопоставления имени метода и определенные аргументы в `SendAsync` и `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="4b18a-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="4b18a-139">Рекомендуется вызывать `connection.start` после `connection.on` , зарегистрированных обработчиков до получения сообщения.</span><span class="sxs-lookup"><span data-stu-id="4b18a-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="4b18a-140">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="4b18a-140">Error handling and logging</span></span>

<span data-ttu-id="4b18a-141">Цепочка `catch` метод в конец `connection.start` метод для обработки ошибок на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4b18a-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="4b18a-142">Используйте `console.error` для ошибок вывода на консоль в браузере.</span><span class="sxs-lookup"><span data-stu-id="4b18a-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=18)]

<span data-ttu-id="4b18a-143">Настройка трассировки журнала клиентской части, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение.</span><span class="sxs-lookup"><span data-stu-id="4b18a-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="4b18a-144">Выводятся сообщения с уровнем указанного журнала и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="4b18a-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="4b18a-145">Существуют следующие уровни доступных журнала.</span><span class="sxs-lookup"><span data-stu-id="4b18a-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="4b18a-146">`signalR.LogLevel.Error` : Сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="4b18a-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="4b18a-147">Журналы `Error` только сообщения.</span><span class="sxs-lookup"><span data-stu-id="4b18a-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="4b18a-148">`signalR.LogLevel.Warning` : Предупреждение сообщения о потенциальных ошибок.</span><span class="sxs-lookup"><span data-stu-id="4b18a-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="4b18a-149">Журналы `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="4b18a-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="4b18a-150">`signalR.LogLevel.Information` : Сообщения о состоянии без ошибок.</span><span class="sxs-lookup"><span data-stu-id="4b18a-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="4b18a-151">Журналы `Information`, `Warning`, и `Error` сообщений.</span><span class="sxs-lookup"><span data-stu-id="4b18a-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="4b18a-152">`signalR.LogLevel.Trace` : Сообщения трассировки.</span><span class="sxs-lookup"><span data-stu-id="4b18a-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="4b18a-153">Заносит в журнал все содержимое, включая данных, перемещаемая между концентратором и клиента.</span><span class="sxs-lookup"><span data-stu-id="4b18a-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="4b18a-154">Передайте средство ведения журнала подключения для начала ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="4b18a-154">Pass the logger to the connection to start logging.</span></span> <span data-ttu-id="4b18a-155">Средства разработчика браузера обычно содержат консоли, где отображаются сообщения.</span><span class="sxs-lookup"><span data-stu-id="4b18a-155">Browser developer tools typically contain a console that displays the messages.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=1-2)]

## <a name="related-resources"></a><span data-ttu-id="4b18a-156">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4b18a-156">Related resources</span></span>

* [<span data-ttu-id="4b18a-157">Концентраторы SignalR в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b18a-157">ASP.NET Core SignalR Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4b18a-158">Включить запросы независимо от источника (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b18a-158">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)