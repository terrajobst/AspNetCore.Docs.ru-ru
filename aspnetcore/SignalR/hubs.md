---
title: Использование концентраторов в ASP.NET Core SignalR
author: rachelappel
description: Сведения об использовании концентраторы в ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 7da0c4832b1aa6a844172bf751a46b280a02f37a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Использование концентраторов в SignalR для ASP.NET Core

По [Рэйчел Аппель](https://twitter.com/rachelappel) и [Kevin Гриффин](https://twitter.com/1kevgriff)

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-a-signalr-hub"></a>Что такое концентратора SignalR

Интерфейс API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера. В серверном коде определить методы, вызываемые клиентом. В коде клиента определить методы, вызываемые с сервера. SignalR обеспечивает все за кулисами, делает возможным обмен данными в режиме реального времени клиент сервер и сервер клиент.

## <a name="configure-signalr-hubs"></a>Настройка концентраторов SignalR

По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

При добавлении SignalR функциональность приложения ASP.NET Core, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a>Создание и использование концентраторов

Создание концентратора путем объявления класса, который наследует от `Hub`и добавить в него открытые методы. Клиенты могут вызывать методы, которые определены как `public`.

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

Можно указать тип возвращаемого значения и параметры, включая сложные типы массивов и, как и в любой метод C#. SignalR обрабатывает сериализация и десериализация сложные объекты и массивы параметров и возвращаемых значений.

## <a name="the-clients-object"></a>Объект клиентов

Каждый экземпляр `Hub` класс имеет свойство с именем `Clients` , содержащий следующие члены для обмена данными между сервером и клиентом:

| Свойство. | Описание |
| ------ | ----------- |
| `All` | Вызывает метод на все подключенные клиенты |
| `Caller` | Вызывает метод на стороне клиента, вызвавшему метод концентратора |
| `Others` | Вызывает метод для всех подключенных клиентов, кроме клиента, который вызывает метод |

Кроме того `Hub` класс содержит следующие методы:

| Метод | Описание |
| ------ | ----------- |
| `AllExcept` | Вызывает метод для всех подключенных клиентов, за исключением указанного соединений |
| `Client` | Вызывает метод для конкретных подключенный клиент |
| `Clients` | Вызывает метод для конкретных подключенные клиенты |
| `Group` | Отправляет сообщение для всех подключений в указанной группе  |
| `GroupExcept` | Отправляет сообщение для всех подключений в указанной группе, за исключением указанного соединения |
| `Groups` | Отправляет сообщение в несколько групп соединений  |
| `OthersInGroup` | Отправляет сообщение в группу подключения, исключая клиента, вызвавшему метод концентратора  |
| `User` | Отправляет сообщение для всех подключений, связанных с конкретным пользователем |
| `Users` | Отправляет сообщение для всех подключений, связанный с указанным пользователям |

Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод. `SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.

## <a name="send-messages-to-clients"></a>Отправлять сообщения для клиентов

Для выполнения вызовов для конкретных клиентов, используйте свойства `Clients` объекта. В следующем в следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора. `SendMessageToGroups` Метод отправляет сообщение в группах, хранящихся в `List` с именем `groups`.

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Обрабатывать события для подключения

Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений. Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к концентратору, такие как добавление в группу.

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a>Обработка ошибок

Исключения, создаваемые в методах hub отправляются клиенту, вызвавшему метод. На стороне клиента JavaScript `invoke` возвращает [объекта обещания JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Когда клиент получает сообщение об ошибке с обработчик присоединяется к promise с помощью `catch`, его вызова, переданного в качестве JavaScript `Error` объекта.

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a>Связанные ресурсы

[Введение в ASP.NET Core SignalR](xref:signalr/introduction)