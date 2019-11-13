---
title: ASP.NET Core SignalR клиента Java
author: mikaelm12
description: Узнайте, как использовать клиент Java ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/java-client
ms.openlocfilehash: d7143b2c22ecdc4e68f484aa4c244e1c520beae0
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963782"
---
# <a name="aspnet-core-opno-locsignalr-java-client"></a><span data-ttu-id="61c20-103">ASP.NET Core SignalR клиента Java</span><span class="sxs-lookup"><span data-stu-id="61c20-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="61c20-104">По [Mikael Менгисту](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="61c20-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="61c20-105">Клиент Java позволяет подключаться к серверу ASP.NET Core SignalR из кода Java, включая приложения Android.</span><span class="sxs-lookup"><span data-stu-id="61c20-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="61c20-106">Как и клиент [JavaScript](xref:signalr/javascript-client) и [клиент .NET](xref:signalr/dotnet-client), клиент Java позволяет получать и отправлять сообщения в концентратор в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="61c20-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="61c20-107">Клиент Java доступен в ASP.NET Core 2,2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="61c20-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="61c20-108">В примере консольного приложения Java, упоминаемого в этой статье, используется клиент SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="61c20-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="61c20-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61c20-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-java-client-package"></a><span data-ttu-id="61c20-110">Установка клиентского пакета SignalR Java</span><span class="sxs-lookup"><span data-stu-id="61c20-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="61c20-111">JAR *-файл 1.0.0* позволяет клиентам подключаться к SignalRным концентраторам.</span><span class="sxs-lookup"><span data-stu-id="61c20-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="61c20-112">Чтобы найти последний номер версии JAR-файла, см. [Результаты поиска Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="61c20-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="61c20-113">При использовании Gradle добавьте следующую строку в раздел `dependencies` файла *Build. Gradle* :</span><span class="sxs-lookup"><span data-stu-id="61c20-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="61c20-114">При использовании Maven добавьте следующие строки в элемент `<dependencies>` файла *POM. XML* :</span><span class="sxs-lookup"><span data-stu-id="61c20-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="61c20-115">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="61c20-115">Connect to a hub</span></span>

<span data-ttu-id="61c20-116">Чтобы установить `HubConnection`, следует использовать `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="61c20-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="61c20-117">URL-адрес центра и уровень журнала можно настроить при создании соединения.</span><span class="sxs-lookup"><span data-stu-id="61c20-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="61c20-118">Настройте все необходимые параметры, вызвав любой из методов `HubConnectionBuilder` перед `build`.</span><span class="sxs-lookup"><span data-stu-id="61c20-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="61c20-119">Запустите подключение с `start`.</span><span class="sxs-lookup"><span data-stu-id="61c20-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="61c20-120">Методы концентратора вызовов от клиента</span><span class="sxs-lookup"><span data-stu-id="61c20-120">Call hub methods from client</span></span>

<span data-ttu-id="61c20-121">Вызов `send` вызывает метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="61c20-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="61c20-122">Передайте имя метода концентратора и все аргументы, определенные в методе Hub, в `send`.</span><span class="sxs-lookup"><span data-stu-id="61c20-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> <span data-ttu-id="61c20-123">Если вы используете службу SignalR Azure в *Бессерверном режиме*, вы не можете вызывать методы концентратора из клиента.</span><span class="sxs-lookup"><span data-stu-id="61c20-123">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="61c20-124">Дополнительные сведения см. в [документации по службеSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="61c20-124">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="61c20-125">Вызов методов клиента из концентратора</span><span class="sxs-lookup"><span data-stu-id="61c20-125">Call client methods from hub</span></span>

<span data-ttu-id="61c20-126">Используйте `hubConnection.on` для определения методов на клиенте, которые может вызывать концентратор.</span><span class="sxs-lookup"><span data-stu-id="61c20-126">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="61c20-127">Определите методы после сборки, но перед запуском соединения.</span><span class="sxs-lookup"><span data-stu-id="61c20-127">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="61c20-128">Добавить ведение журнала</span><span class="sxs-lookup"><span data-stu-id="61c20-128">Add logging</span></span>

<span data-ttu-id="61c20-129">Клиент SignalR Java использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="61c20-129">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="61c20-130">Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="61c20-130">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="61c20-131">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="61c20-131">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="61c20-132">Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:</span><span class="sxs-lookup"><span data-stu-id="61c20-132">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="61c20-133">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="61c20-133">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="61c20-134">Заметки о разработке для Android</span><span class="sxs-lookup"><span data-stu-id="61c20-134">Android development notes</span></span>

<span data-ttu-id="61c20-135">В отношении пакет SDK для Android совместимости для функций клиента SignalR при указании целевой пакет SDK для Android необходимо учитывать следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="61c20-135">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="61c20-136">Клиент SignalR Java будет работать на API Android уровня 16 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="61c20-136">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="61c20-137">Для подключения через службу SignalR Azure потребуется уровень API Android 20 и более поздней версии, так как [служба SignalR Azure](/azure/azure-signalr/signalr-overview) требует TLS 1,2 и не поддерживает комплекты шифров на основе SHA-1.</span><span class="sxs-lookup"><span data-stu-id="61c20-137">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="61c20-138">В Android [добавлена поддержка комплектов шифров SHA-256 (и более поздних версий)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) на уровне API 20.</span><span class="sxs-lookup"><span data-stu-id="61c20-138">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="61c20-139">Настройка проверки подлинности токена носителя</span><span class="sxs-lookup"><span data-stu-id="61c20-139">Configure bearer token authentication</span></span>

<span data-ttu-id="61c20-140">В клиенте SignalR Java можно настроить токен носителя для использования при проверке подлинности, предоставив "фабрику маркеров доступа" для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="61c20-140">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="61c20-141">Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [Рксжава](https://github.com/ReactiveX/RxJava) [одно\<String >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="61c20-141">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="61c20-142">При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.</span><span class="sxs-lookup"><span data-stu-id="61c20-142">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="61c20-143">Известные ограничения</span><span class="sxs-lookup"><span data-stu-id="61c20-143">Known limitations</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="61c20-144">Поддерживается только протокол JSON.</span><span class="sxs-lookup"><span data-stu-id="61c20-144">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="61c20-145">Транспортировка транспорта и транспорт событий отправки сервера не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="61c20-145">Transport fallback and the Server Sent Events transport aren't supported.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="61c20-146">Поддерживается только протокол JSON.</span><span class="sxs-lookup"><span data-stu-id="61c20-146">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="61c20-147">Поддерживается только транспорт WebSockets.</span><span class="sxs-lookup"><span data-stu-id="61c20-147">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="61c20-148">Потоковая передача еще не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="61c20-148">Streaming isn't supported yet.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="61c20-149">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="61c20-149">Additional resources</span></span>

* [<span data-ttu-id="61c20-150">Справочник по API Java</span><span class="sxs-lookup"><span data-stu-id="61c20-150">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* <span data-ttu-id="61c20-151">[Документация по Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="61c20-151">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
