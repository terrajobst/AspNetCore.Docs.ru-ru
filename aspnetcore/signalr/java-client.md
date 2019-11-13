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
# <a name="aspnet-core-opno-locsignalr-java-client"></a>ASP.NET Core SignalR клиента Java

По [Mikael Менгисту](https://twitter.com/MikaelM_12)

Клиент Java позволяет подключаться к серверу ASP.NET Core SignalR из кода Java, включая приложения Android. Как и клиент [JavaScript](xref:signalr/javascript-client) и [клиент .NET](xref:signalr/dotnet-client), клиент Java позволяет получать и отправлять сообщения в концентратор в режиме реального времени. Клиент Java доступен в ASP.NET Core 2,2 и более поздних версиях.

В примере консольного приложения Java, упоминаемого в этой статье, используется клиент SignalR Java.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-java-client-package"></a>Установка клиентского пакета SignalR Java

JAR *-файл 1.0.0* позволяет клиентам подключаться к SignalRным концентраторам. Чтобы найти последний номер версии JAR-файла, см. [Результаты поиска Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

При использовании Gradle добавьте следующую строку в раздел `dependencies` файла *Build. Gradle* :

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

При использовании Maven добавьте следующие строки в элемент `<dependencies>` файла *POM. XML* :

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить `HubConnection`, следует использовать `HubConnectionBuilder`. URL-адрес центра и уровень журнала можно настроить при создании соединения. Настройте все необходимые параметры, вызвав любой из методов `HubConnectionBuilder` перед `build`. Запустите подключение с `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Методы концентратора вызовов от клиента

Вызов `send` вызывает метод концентратора. Передайте имя метода концентратора и все аргументы, определенные в методе Hub, в `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Если вы используете службу SignalR Azure в *Бессерверном режиме*, вы не можете вызывать методы концентратора из клиента. Дополнительные сведения см. в [документации по службеSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Вызов методов клиента из концентратора

Используйте `hubConnection.on` для определения методов на клиенте, которые может вызывать концентратор. Определите методы после сборки, но перед запуском соединения.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Добавить ведение журнала

Клиент SignalR Java использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала. Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала. В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом Java SignalR.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Это можно игнорировать.

## <a name="android-development-notes"></a>Заметки о разработке для Android

В отношении пакет SDK для Android совместимости для функций клиента SignalR при указании целевой пакет SDK для Android необходимо учитывать следующие элементы:

* Клиент SignalR Java будет работать на API Android уровня 16 и более поздних версий.
* Для подключения через службу SignalR Azure потребуется уровень API Android 20 и более поздней версии, так как [служба SignalR Azure](/azure/azure-signalr/signalr-overview) требует TLS 1,2 и не поддерживает комплекты шифров на основе SHA-1. В Android [добавлена поддержка комплектов шифров SHA-256 (и более поздних версий)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) на уровне API 20.

## <a name="configure-bearer-token-authentication"></a>Настройка проверки подлинности токена носителя

В клиенте SignalR Java можно настроить токен носителя для использования при проверке подлинности, предоставив "фабрику маркеров доступа" для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [Рксжава](https://github.com/ReactiveX/RxJava) [одно\<String >](https://reactivex.io/documentation/single.html). При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Известные ограничения

::: moniker range=">= aspnetcore-3.0"

* Поддерживается только протокол JSON.
* Транспортировка транспорта и транспорт событий отправки сервера не поддерживаются.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Поддерживается только протокол JSON.
* Поддерживается только транспорт WebSockets.
* Потоковая передача еще не поддерживается.

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Справочник по API Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Документация по Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
