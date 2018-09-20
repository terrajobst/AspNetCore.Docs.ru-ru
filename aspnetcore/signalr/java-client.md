---
title: ASP.NET Core SignalR Java-клиент
author: mikaelm12
description: Узнайте, как использовать клиент ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482922"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="36913-103">Клиент SignalR Java ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36913-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="36913-104">По [Майкл Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="36913-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="36913-105">Клиент Java обеспечивает соединение с сервером ASP.NET Core SignalR из кода Java, включая приложения Android.</span><span class="sxs-lookup"><span data-stu-id="36913-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="36913-106">Как и [клиента JavaScript](xref:signalr/javascript-client) и [клиента .NET](xref:signalr/dotnet-client), клиент Java позволяет получать и отправлять сообщения в центр в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="36913-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="36913-107">Клиент Java в ASP.NET Core 2.2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="36913-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="36913-108">Консольное приложение Java пример, в этой статье использует клиент SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="36913-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="36913-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="36913-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="36913-110">Установка пакета клиента SignalR Java</span><span class="sxs-lookup"><span data-stu-id="36913-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="36913-111">*Signalr-0.1.0-имеет предварительную версию 2-35174* JAR-файл позволяет клиентам подключаться к концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="36913-111">The *signalr-0.1.0-preview2-35174* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="36913-112">Чтобы найти номер последней версии файла JAR-ФАЙЛ, см. в разделе [результаты поиска Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="36913-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="36913-113">Если с помощью Gradle, добавьте следующую строку для `dependencies` часть вашей *build.gradle* файла:</span><span class="sxs-lookup"><span data-stu-id="36913-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

<span data-ttu-id="36913-114">Если с помощью Maven, добавьте следующие строки внутри `<dependencies>` элемент вашей *pom.xml* файла:</span><span class="sxs-lookup"><span data-stu-id="36913-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="36913-115">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="36913-115">Connect to a hub</span></span>

<span data-ttu-id="36913-116">Чтобы установить эту `HubConnection`, `HubConnectionBuilder` следует использовать.</span><span class="sxs-lookup"><span data-stu-id="36913-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="36913-117">Уровень концентратора URL-адрес и журнала можно настроить при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="36913-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="36913-118">Настройте любые необходимые параметры при вызовах `HubConnectionBuilder` методы перед `build`.</span><span class="sxs-lookup"><span data-stu-id="36913-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="36913-119">Установить подключение с `start`.</span><span class="sxs-lookup"><span data-stu-id="36913-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="36913-120">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="36913-120">Call hub methods from client</span></span>

<span data-ttu-id="36913-121">Вызов `send` вызывающая метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="36913-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="36913-122">Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `send`.</span><span class="sxs-lookup"><span data-stu-id="36913-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="36913-123">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="36913-123">Call client methods from hub</span></span>

<span data-ttu-id="36913-124">Используйте `hubConnection.on` для определения методов на стороне клиента, который может вызывать концентратора.</span><span class="sxs-lookup"><span data-stu-id="36913-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="36913-125">Определите методы после сборки, но прежде чем выполнять подключение.</span><span class="sxs-lookup"><span data-stu-id="36913-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="36913-126">Известные ограничения</span><span class="sxs-lookup"><span data-stu-id="36913-126">Known limitations</span></span>

<span data-ttu-id="36913-127">Это ранняя Предварительная версия клиента Java.</span><span class="sxs-lookup"><span data-stu-id="36913-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="36913-128">Существует множество функций, которые пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="36913-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="36913-129">Следующие пропусков которой вы работаете в будущие выпуски:</span><span class="sxs-lookup"><span data-stu-id="36913-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="36913-130">Только типы-примитивы могут быть приняты в качестве параметров и возвращаемых типов.</span><span class="sxs-lookup"><span data-stu-id="36913-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="36913-131">API-интерфейсы являются синхронными.</span><span class="sxs-lookup"><span data-stu-id="36913-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="36913-132">В настоящее время поддерживается только тип вызова «Отправить».</span><span class="sxs-lookup"><span data-stu-id="36913-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="36913-133">«Вызов» и потоковую передачу возвращаемых значений, не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="36913-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="36913-134">Поддерживается только протокол JSON.</span><span class="sxs-lookup"><span data-stu-id="36913-134">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="36913-135">Поддерживается только транспорт WebSockets.</span><span class="sxs-lookup"><span data-stu-id="36913-135">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36913-136">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="36913-136">Additional resources</span></span>

* [<span data-ttu-id="36913-137">Справочник по Java API</span><span class="sxs-lookup"><span data-stu-id="36913-137">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
