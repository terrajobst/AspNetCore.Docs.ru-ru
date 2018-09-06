---
title: ASP.NET Core SignalR Java-клиент
author: mikaelm12
description: Узнайте, как использовать клиент ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: f110f5391ac34f5cb4a72f64c16d86c8a37369a2
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2018
ms.locfileid: "43995422"
---
# <a name="aspnet-core-signalr-java-client"></a>Клиент SignalR Java ASP.NET Core

По [Майкл Mengistu](https://twitter.com/MikaelM_12)

Клиент Java обеспечивает соединение с сервером ASP.NET Core SignalR из кода Java, включая приложения Android. Как и [клиента JavaScript](xref:signalr/javascript-client) и [клиента .NET](xref:signalr/dotnet-client), клиент Java позволяет получать и отправлять сообщения в центр в режиме реального времени. Клиент Java в ASP.NET Core 2.2 и более поздних версий.

Консольное приложение Java пример, в этой статье использует клиент SignalR Java.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Установка пакета клиента SignalR Java

*Signalr 0.1.0-Предварительная версия 1 35029* JAR-файл позволяет клиентам подключаться к концентраторов SignalR. Чтобы найти номер последней версии файла JAR-ФАЙЛ, см. в разделе [результаты поиска Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Если с помощью Gradle, добавьте следующую строку для `dependencies` часть вашей *build.gradle* файла:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

Если с помощью Maven, добавьте следующие строки внутри `<dependencies>` элемент вашей *pom.xml* файла:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить эту `HubConnection`, `HubConnectionBuilder` следует использовать. Уровень концентратора URL-адрес и журнала можно настроить при создании подключения. Настройте любые необходимые параметры при вызовах `HubConnectionBuilder` методы перед `build`. Установить подключение с `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

Вызов `send` вызывающая метод концентратора. Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Используйте `hubConnection.on` для определения методов на стороне клиента, который может вызывать концентратора. Определите методы после сборки, но прежде чем выполнять подключение.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Известные ограничения

Это ранняя Предварительная версия клиента Java. Существует множество функций, которые пока не поддерживаются. Следующие пропусков которой вы работаете в будущие выпуски:

* Только типы-примитивы могут быть приняты в качестве параметров и возвращаемых типов.
* API-интерфейсы являются синхронными.
* В настоящее время поддерживается только тип вызова «Отправить». «Вызов» и потоковую передачу возвращаемых значений, не поддерживаются.
* В настоящее время не поддерживает клиент [служба Azure SignalR](/azure/azure-signalr/).
* Поддерживается только протокол JSON.
* Поддерживается только транспорт WebSockets.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
