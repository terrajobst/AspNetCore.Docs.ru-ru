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
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="3677f-103">Управление пользователями и группами в SignalR</span><span class="sxs-lookup"><span data-stu-id="3677f-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="3677f-104">По [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="3677f-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="3677f-105">SignalR позволяет сообщения будут отправляться все соединения, связанные с конкретным пользователем, а также с именем группы подключений.</span><span class="sxs-lookup"><span data-stu-id="3677f-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="3677f-106">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(как для загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3677f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="3677f-107">Пользователи в SignalR</span><span class="sxs-lookup"><span data-stu-id="3677f-107">Users in SignalR</span></span>

<span data-ttu-id="3677f-108">SignalR позволяет отправлять сообщения для всех подключений, связанных с конкретным пользователем.</span><span class="sxs-lookup"><span data-stu-id="3677f-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="3677f-109">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением с идентификатором пользователя.</span><span class="sxs-lookup"><span data-stu-id="3677f-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="3677f-110">Один пользователь может иметь несколько подключений к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="3677f-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="3677f-111">Например пользователь может подключение на рабочем столе, а также свой телефон.</span><span class="sxs-lookup"><span data-stu-id="3677f-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="3677f-112">Каждое устройство имеет отдельное соединение SignalR, но все они связаны с тем же пользователем.</span><span class="sxs-lookup"><span data-stu-id="3677f-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="3677f-113">Если сообщение отправляется пользователю, все подключения, связанные с этим пользователем получит сообщение.</span><span class="sxs-lookup"><span data-stu-id="3677f-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="3677f-114">Отправить сообщение для пользователя, передавая идентификатор пользователя для `User` функции в методе концентратора, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="3677f-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="3677f-115">Идентификатор пользователя с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="3677f-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="3677f-116">Идентификатор пользователя можно настраивать путем создания `IUserIdProvider`и его регистрации в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3677f-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="3677f-117">AddSignalR должен быть вызван до регистрации пользовательских служб SignalR.</span><span class="sxs-lookup"><span data-stu-id="3677f-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="3677f-118">Группы в SignalR</span><span class="sxs-lookup"><span data-stu-id="3677f-118">Groups in SignalR</span></span>

<span data-ttu-id="3677f-119">Группа — это коллекция соединений, связанный с именем.</span><span class="sxs-lookup"><span data-stu-id="3677f-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="3677f-120">Сообщения могут отправляться для всех подключений в группе.</span><span class="sxs-lookup"><span data-stu-id="3677f-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="3677f-121">Группы — рекомендуемый способ отправить несколько подключений или подключения, поскольку группы управляются с помощью приложения.</span><span class="sxs-lookup"><span data-stu-id="3677f-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="3677f-122">Соединение может быть членом нескольких групп.</span><span class="sxs-lookup"><span data-stu-id="3677f-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="3677f-123">Это делает групп идеальной для примерно приложения разговора, где каждой комнате, можно представить в виде группы.</span><span class="sxs-lookup"><span data-stu-id="3677f-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="3677f-124">Соединения можно добавлены или удалены из групп через `AddToGroupAsync` и `RemoveFromGroupAsync` методы.</span><span class="sxs-lookup"><span data-stu-id="3677f-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="3677f-125">Членство в группе не сохраняется при повторном подключении.</span><span class="sxs-lookup"><span data-stu-id="3677f-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="3677f-126">Подключение необходимо повторно подключиться к группе при ее заново установить.</span><span class="sxs-lookup"><span data-stu-id="3677f-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="3677f-127">Число членов группы, так как эта информация недоступна, если приложение изменяется на несколько серверов невозможна.</span><span class="sxs-lookup"><span data-stu-id="3677f-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="3677f-128">Группы в именах учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="3677f-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3677f-129">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3677f-129">Related resources</span></span>

* [<span data-ttu-id="3677f-130">Начало работы</span><span class="sxs-lookup"><span data-stu-id="3677f-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="3677f-131">Центры</span><span class="sxs-lookup"><span data-stu-id="3677f-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3677f-132">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="3677f-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
