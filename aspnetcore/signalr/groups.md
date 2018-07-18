---
title: Управление пользователями и группами в SignalR
author: tdykstra
description: Общие сведения о ASP.NET Core SignalR пользователей и групп управления.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d54ab2a113345f98e26425a88cad165d67b8d456
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095025"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="9b4a9-103">Управление пользователями и группами в SignalR</span><span class="sxs-lookup"><span data-stu-id="9b4a9-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="9b4a9-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="9b4a9-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="9b4a9-105">SignalR позволяет сообщения отправляться на все подключения, связанные с конкретным пользователем, а также с именем группы подключений.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="9b4a9-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="9b4a9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="9b4a9-107">Пользователи в SignalR</span><span class="sxs-lookup"><span data-stu-id="9b4a9-107">Users in SignalR</span></span>

<span data-ttu-id="9b4a9-108">SignalR позволяет отправлять сообщения для всех подключений, связанных с конкретным пользователем.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="9b4a9-109">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="9b4a9-110">Один пользователь может иметь несколько подключений к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="9b4a9-111">Например пользователь может подключаться на их рабочем столе, а также свой телефон.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="9b4a9-112">Каждое устройство имеет отдельное соединение SignalR, но они все связанные с тем же пользователем.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="9b4a9-113">Если сообщение отправляется пользователю, все подключения, связанный с данным пользователем сообщение.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="9b4a9-114">Идентификатор пользователя для подключения может осуществляться `Context.UserIdentifier` свойство в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="9b4a9-115">Отправить сообщение конкретному пользователю, передав идентификатор пользователя для `User` функции в методе концентратора, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="9b4a9-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="9b4a9-116">Идентификатор пользователя с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="9b4a9-117">Идентификатор пользователя можно настроить, создав `IUserIdProvider`и его регистрации в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="9b4a9-118">AddSignalR должен вызываться перед регистрацией пользовательских служб SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="9b4a9-119">Группами в SignalR</span><span class="sxs-lookup"><span data-stu-id="9b4a9-119">Groups in SignalR</span></span>

<span data-ttu-id="9b4a9-120">Группа — коллекция соединений, связанный с именем.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="9b4a9-121">Сообщения могут отправляться для всех подключений в группе.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="9b4a9-122">Группы — рекомендуемый способ отправить подключения или несколько подключений, так как группы управляются приложением.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="9b4a9-123">Соединение может быть членом нескольких групп.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="9b4a9-124">Это делает групп идеальной для примерно приложения чата, где каждой комнате, можно представить в виде группы.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="9b4a9-125">Соединения можно добавлять к или удалены из групп с помощью `AddToGroupAsync` и `RemoveFromGroupAsync` методы.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="9b4a9-126">Членство в группе не сохраняется при повторном подключении.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="9b4a9-127">Подключение должно повторно присоединиться к группе, когда она будет восстановлена.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="9b4a9-128">Число членов группы, так как эта информация недоступна, если приложение масштабируется на несколько серверов невозможна.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="9b4a9-129">Имена групп учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="9b4a9-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="9b4a9-130">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9b4a9-130">Related resources</span></span>

* [<span data-ttu-id="9b4a9-131">Начало работы</span><span class="sxs-lookup"><span data-stu-id="9b4a9-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="9b4a9-132">Центры</span><span class="sxs-lookup"><span data-stu-id="9b4a9-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9b4a9-133">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="9b4a9-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
