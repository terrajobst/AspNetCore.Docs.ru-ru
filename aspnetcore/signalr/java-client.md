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
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java-клиент

По [Майкл Mengistu](https://twitter.com/MikaelM_12)

Клиент Java обеспечивает соединение с сервером ASP.NET Core SignalR из кода Java, включая приложения Android. Как и [клиента JavaScript](xref:signalr/javascript-client) и [клиента .NET](xref:signalr/dotnet-client), клиент Java позволяет получать и отправлять сообщения в центр в режиме реального времени. Клиент Java в ASP.NET Core 2.2 и более поздних версий.

Консольное приложение Java пример, в этой статье использует клиент SignalR Java.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Установка пакета клиента SignalR Java

*Signalr 1.0.0-preview3 35501* JAR-файл позволяет клиентам подключаться к концентраторов SignalR. Чтобы найти номер последней версии файла JAR-ФАЙЛ, см. в разделе [результаты поиска Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Если с помощью Gradle, добавьте следующую строку для `dependencies` часть вашей *build.gradle* файла:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Если с помощью Maven, добавьте следующие строки внутри `<dependencies>` элемент вашей *pom.xml* файла:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить эту `HubConnection`, `HubConnectionBuilder` следует использовать. Уровень концентратора URL-адрес и журнала можно настроить при создании подключения. Настройте любые необходимые параметры при вызовах `HubConnectionBuilder` методы перед `build`. Установить подключение с `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

Вызов `send` вызывающая метод концентратора. Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Используйте `hubConnection.on` для определения методов на стороне клиента, который может вызывать концентратора. Определите методы после сборки, но прежде чем выполнять подключение.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Добавление ведения журнала

Клиент SignalR Java использует [SLF4J](https://www.slf4j.org/) библиотеки для ведения журнала. Это API высокого уровня ведения журнала, который позволяет пользователям библиотеки выбрал свою собственную реализацию определенного ведения журнала, Собрав воедино в зависимость от конкретных ведения журнала. В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Если не настроить ведение журнала в зависимости, SLF4J загружает средство ведения журнала по умолчанию без операции с следующее предупреждающее сообщение:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Это можно игнорировать.


## <a name="configure-bearer-token-authentication"></a>Настройка проверки подлинности маркера носителя

В клиенте SignalR Java, можно настроить токен носителя для проверки подлинности, предоставляя «доступ маркера фабрику», чтобы [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Используйте [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [RxJava](https://github.com/ReactiveX/RxJava) [единый<String>](http://reactivex.io/documentation/single.html). С помощью вызова [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), можно написать логику для получения маркеров доступа для вашего клиента.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Известные ограничения

Это предварительная версия клиента Java. Некоторые функции не поддерживаются:

* Поддерживается только протокол JSON.
* Поддерживается только транспорт WebSockets.
* Потоковая передача еще не поддерживается.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Справочник по API Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
