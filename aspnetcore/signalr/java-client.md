---
title: ASP.NET Core SignalR Java-клиент
author: mikaelm12
description: Узнайте, как использовать клиент ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/java-client
ms.openlocfilehash: 4ee4e61fc301ebeec4d95b1167f94f16c38f3ac5
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225425"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="ab24a-103">ASP.NET Core SignalR Java-клиент</span><span class="sxs-lookup"><span data-stu-id="ab24a-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="ab24a-104">По [Майкл Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="ab24a-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="ab24a-105">Клиент Java обеспечивает соединение с сервером ASP.NET Core SignalR из кода Java, включая приложения Android.</span><span class="sxs-lookup"><span data-stu-id="ab24a-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="ab24a-106">Как и [клиента JavaScript](xref:signalr/javascript-client) и [клиента .NET](xref:signalr/dotnet-client), клиент Java позволяет получать и отправлять сообщения в центр в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="ab24a-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="ab24a-107">Клиент Java в ASP.NET Core 2.2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="ab24a-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="ab24a-108">Консольное приложение Java пример, в этой статье использует клиент SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="ab24a-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="ab24a-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab24a-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="ab24a-110">Установка пакета клиента SignalR Java</span><span class="sxs-lookup"><span data-stu-id="ab24a-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="ab24a-111">*Signalr 1.0.0-preview3 35501* JAR-файл позволяет клиентам подключаться к концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab24a-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ab24a-112">Чтобы найти номер последней версии файла JAR-ФАЙЛ, см. в разделе [результаты поиска Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="ab24a-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="ab24a-113">Если с помощью Gradle, добавьте следующую строку для `dependencies` часть вашей *build.gradle* файла:</span><span class="sxs-lookup"><span data-stu-id="ab24a-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="ab24a-114">Если с помощью Maven, добавьте следующие строки внутри `<dependencies>` элемент вашей *pom.xml* файла:</span><span class="sxs-lookup"><span data-stu-id="ab24a-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="ab24a-115">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="ab24a-115">Connect to a hub</span></span>

<span data-ttu-id="ab24a-116">Чтобы установить эту `HubConnection`, `HubConnectionBuilder` следует использовать.</span><span class="sxs-lookup"><span data-stu-id="ab24a-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="ab24a-117">Уровень концентратора URL-адрес и журнала можно настроить при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="ab24a-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="ab24a-118">Настройте любые необходимые параметры при вызовах `HubConnectionBuilder` методы перед `build`.</span><span class="sxs-lookup"><span data-stu-id="ab24a-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="ab24a-119">Установить подключение с `start`.</span><span class="sxs-lookup"><span data-stu-id="ab24a-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ab24a-120">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="ab24a-120">Call hub methods from client</span></span>

<span data-ttu-id="ab24a-121">Вызов `send` вызывающая метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="ab24a-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="ab24a-122">Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `send`.</span><span class="sxs-lookup"><span data-stu-id="ab24a-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ab24a-123">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="ab24a-123">Call client methods from hub</span></span>

<span data-ttu-id="ab24a-124">Используйте `hubConnection.on` для определения методов на стороне клиента, который может вызывать концентратора.</span><span class="sxs-lookup"><span data-stu-id="ab24a-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="ab24a-125">Определите методы после сборки, но прежде чем выполнять подключение.</span><span class="sxs-lookup"><span data-stu-id="ab24a-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="ab24a-126">Добавление ведения журнала</span><span class="sxs-lookup"><span data-stu-id="ab24a-126">Add logging</span></span>

<span data-ttu-id="ab24a-127">Клиент SignalR Java использует [SLF4J](https://www.slf4j.org/) библиотеки для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="ab24a-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ab24a-128">Это API высокого уровня ведения журнала, который позволяет пользователям библиотеки выбрал свою собственную реализацию определенного ведения журнала, Собрав воедино в зависимость от конкретных ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="ab24a-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ab24a-129">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="ab24a-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ab24a-130">Если не настроить ведение журнала в зависимости, SLF4J загружает средство ведения журнала по умолчанию без операции с следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="ab24a-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ab24a-131">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="ab24a-131">This can safely be ignored.</span></span>


## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="ab24a-132">Настройка проверки подлинности маркера носителя</span><span class="sxs-lookup"><span data-stu-id="ab24a-132">Configure bearer token authentication</span></span>

<span data-ttu-id="ab24a-133">В клиенте SignalR Java, можно настроить токен носителя для проверки подлинности, предоставляя «доступ маркера фабрику», чтобы [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="ab24a-133">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ab24a-134">Используйте [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [RxJava](https://github.com/ReactiveX/RxJava) [единый<String>](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="ab24a-134">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ab24a-135">С помощью вызова [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), можно написать логику для получения маркеров доступа для вашего клиента.</span><span class="sxs-lookup"><span data-stu-id="ab24a-135">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="ab24a-136">Известные ограничения</span><span class="sxs-lookup"><span data-stu-id="ab24a-136">Known limitations</span></span>

<span data-ttu-id="ab24a-137">Это предварительная версия клиента Java.</span><span class="sxs-lookup"><span data-stu-id="ab24a-137">This is a preview release of the Java client.</span></span> <span data-ttu-id="ab24a-138">Некоторые функции не поддерживаются:</span><span class="sxs-lookup"><span data-stu-id="ab24a-138">Some features aren't supported:</span></span>

* <span data-ttu-id="ab24a-139">Поддерживается только протокол JSON.</span><span class="sxs-lookup"><span data-stu-id="ab24a-139">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="ab24a-140">Поддерживается только транспорт WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ab24a-140">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="ab24a-141">Потоковая передача еще не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="ab24a-141">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab24a-142">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ab24a-142">Additional resources</span></span>

* [<span data-ttu-id="ab24a-143">Справочник по API Java</span><span class="sxs-lookup"><span data-stu-id="ab24a-143">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
