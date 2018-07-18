---
title: Использование центров в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать центры в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095285"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Использование центров в SignalR для ASP.NET Core

По [Рейчел Аппель](https://twitter.com/rachelappel) и [Кевин Гриффин](https://twitter.com/1kevgriff)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Что такое концентратор SignalR

API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера. В серверном коде определить методы, вызываемые клиентом. В клиентском коде определить методы, вызываемые с сервера. SignalR отвечает за все за кулисами, позволяет в режиме реального времени связи клиент сервер и сервер клиент.

## <a name="configure-signalr-hubs"></a>Настройка концентраторов SignalR

По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

При добавлении приложения ASP.NET Core SignalR функциональные возможности, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Создание и использование концентраторов

Создать концентратор, объявляя класс, наследуемый от `Hub`и добавьте в него открытые методы. Клиенты могут вызывать методы, которые определены как `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#. SignalR обрабатывает сериализации и десериализации сложных объектов и массивов в параметрах и возвращаемых значений.

## <a name="the-clients-object"></a>Объект клиентов

Каждый экземпляр `Hub` класс имеет свойство с именем `Clients` , содержащий следующие элементы для обмена данными между сервером и клиентом:

| Свойство. | Описание: |
| ------ | ----------- |
| `All` | Вызывает метод на все подключенные клиенты |
| `Caller` | Вызывает метод на стороне клиента, вызвавшему метод концентратора |
| `Others` | Вызывает метод на все подключенные клиенты, кроме клиента, который вызывает метод |


Кроме того `Hub.Clients` содержит следующие методы:

| Метод | Описание: |
| ------ | ----------- |
| `AllExcept` | Вызывает метод на все подключенные клиенты, за исключением указанного соединений |
| `Client` | Вызывает метод для определенного подключенного клиента |
| `Clients` | Вызывает метод для конкретных подключенных клиентов |
| `Group` | Вызывает метод для всех подключений в указанной группе  |
| `GroupExcept` | Вызывает метод для всех подключений, в состав указанной группы, за исключением указанным подключениям |
| `Groups` | Вызывает метод в несколько групп соединений  |
| `OthersInGroup` | Вызывает метод для группы соединений, за исключением клиента, вызвавшему метод концентратора  |
| `User` | Вызывает метод для всех подключений, связанных с конкретным пользователем |
| `Users` | Вызывает метод для всех подключений, связанных с определенными пользователями |

Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод. `SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.

## <a name="send-messages-to-clients"></a>Отправить сообщения клиентам

Чтобы выполнять вызовы для конкретных клиентов, используйте свойства `Clients` объекта. В следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора. `SendMessageToGroups` Метод отправляет сообщение в группы, хранящиеся в `List` с именем `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Обработка событий для подключения

Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений. Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к центру, таких как добавление его в группу.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Обработка ошибок

Исключения, возникшие в методах hub отправляются клиенту, вызвавшему метод. На стороне клиента JavaScript `invoke` возвращает [обещание JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Когда клиент получает ошибку с обработчиком подключенных к promise с помощью `catch`, он вызывается и передан в качестве JavaScript `Error` объекта.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Связанные ресурсы

* [Введение в ASP.NET Core SignalR](xref:signalr/introduction)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
