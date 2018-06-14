---
title: Управление пользователями и группами в SignalR
author: rachelappel
description: Общие сведения об управлении ASP.NET Core SignalR пользователей и групп.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358450"
---
# <a name="manage-users-and-groups-in-signalr"></a>Управление пользователями и группами в SignalR

По [Brennan Conroy](https://github.com/BrennanConroy)

SignalR позволяет сообщения будут отправляться все соединения, связанные с конкретным пользователем, а также с именем группы подключений.

[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(как для загрузки)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Пользователи в SignalR

SignalR позволяет отправлять сообщения для всех подключений, связанных с конкретным пользователем. По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением с идентификатором пользователя. Один пользователь может иметь несколько подключений к приложению SignalR. Например пользователь может подключение на рабочем столе, а также свой телефон. Каждое устройство имеет отдельное соединение SignalR, но все они связаны с тем же пользователем. Если сообщение отправляется пользователю, все подключения, связанные с этим пользователем получит сообщение.

Отправить сообщение для пользователя, передавая идентификатор пользователя для `User` функции в методе концентратора, как показано в следующем примере:

> [!NOTE]
> Идентификатор пользователя с учетом регистра.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Идентификатор пользователя можно настраивать путем создания `IUserIdProvider`и его регистрации в `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR должен быть вызван до регистрации пользовательских служб SignalR.

## <a name="groups-in-signalr"></a>Группы в SignalR

Группа — это коллекция соединений, связанный с именем. Сообщения могут отправляться для всех подключений в группе. Группы — рекомендуемый способ отправить несколько подключений или подключения, поскольку группы управляются с помощью приложения. Соединение может быть членом нескольких групп. Это делает групп идеальной для примерно приложения разговора, где каждой комнате, можно представить в виде группы. Соединения можно добавлены или удалены из групп через `AddToGroupAsync` и `RemoveFromGroupAsync` методы.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Членство в группе не сохраняется при повторном подключении. Подключение необходимо повторно подключиться к группе при ее заново установить. Число членов группы, так как эта информация недоступна, если приложение изменяется на несколько серверов невозможна.

> [!NOTE]
> Группы в именах учитывается регистр.

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:signalr/get-started)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
