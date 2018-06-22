---
title: Управление пользователями и группами в SignalR
author: rachelappel
description: Общие сведения об управлении ASP.NET Core SignalR пользователей и групп.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272085"
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

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
