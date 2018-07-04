---
title: Управление пользователями и группами в SignalR
author: rachelappel
description: Общие сведения о ASP.NET Core SignalR пользователей и групп управления.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402529"
---
# <a name="manage-users-and-groups-in-signalr"></a>Управление пользователями и группами в SignalR

По [Бреннан Конрой](https://github.com/BrennanConroy)

SignalR позволяет сообщения отправляться на все подключения, связанные с конкретным пользователем, а также с именем группы подключений.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Пользователи в SignalR

SignalR позволяет отправлять сообщения для всех подключений, связанных с конкретным пользователем. По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя. Один пользователь может иметь несколько подключений к приложению SignalR. Например пользователь может подключаться на их рабочем столе, а также свой телефон. Каждое устройство имеет отдельное соединение SignalR, но они все связанные с тем же пользователем. Если сообщение отправляется пользователю, все подключения, связанный с данным пользователем сообщение. Идентификатор пользователя для подключения может осуществляться `Context.UserIdentifier` свойство в концентраторе.

Отправить сообщение конкретному пользователю, передав идентификатор пользователя для `User` функции в методе концентратора, как показано в следующем примере:

> [!NOTE]
> Идентификатор пользователя с учетом регистра.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Идентификатор пользователя можно настроить, создав `IUserIdProvider`и его регистрации в `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR должен вызываться перед регистрацией пользовательских служб SignalR.

## <a name="groups-in-signalr"></a>Группами в SignalR

Группа — коллекция соединений, связанный с именем. Сообщения могут отправляться для всех подключений в группе. Группы — рекомендуемый способ отправить подключения или несколько подключений, так как группы управляются приложением. Соединение может быть членом нескольких групп. Это делает групп идеальной для примерно приложения чата, где каждой комнате, можно представить в виде группы. Соединения можно добавлять к или удалены из групп с помощью `AddToGroupAsync` и `RemoveFromGroupAsync` методы.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Членство в группе не сохраняется при повторном подключении. Подключение должно повторно присоединиться к группе, когда она будет восстановлена. Число членов группы, так как эта информация недоступна, если приложение масштабируется на несколько серверов невозможна.

> [!NOTE]
> Имена групп учитывается регистр.

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
