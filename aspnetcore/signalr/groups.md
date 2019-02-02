---
title: Управление пользователями и группами в SignalR
author: bradygaster
description: Общие сведения о ASP.NET Core SignalR пользователей и групп управления.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667756"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="7c2ed-103">Управление пользователями и группами в SignalR</span><span class="sxs-lookup"><span data-stu-id="7c2ed-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="7c2ed-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="7c2ed-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="7c2ed-105">SignalR позволяет сообщения отправляться на все подключения, связанные с конкретным пользователем, а также с именем группы подключений.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="7c2ed-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7c2ed-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="7c2ed-107">Пользователи в SignalR</span><span class="sxs-lookup"><span data-stu-id="7c2ed-107">Users in SignalR</span></span>

<span data-ttu-id="7c2ed-108">SignalR позволяет отправлять сообщения для всех подключений, связанных с конкретным пользователем.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="7c2ed-109">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="7c2ed-110">Один пользователь может иметь несколько подключений к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="7c2ed-111">Например пользователь может подключаться на их рабочем столе, а также свой телефон.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="7c2ed-112">Каждое устройство имеет отдельное соединение SignalR, но они все связанные с тем же пользователем.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="7c2ed-113">Если сообщение отправляется пользователю, все подключения, связанный с данным пользователем сообщение.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="7c2ed-114">Идентификатор пользователя для подключения может осуществляться `Context.UserIdentifier` свойство в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="7c2ed-115">Отправить сообщение конкретному пользователю, передав идентификатор пользователя для `User` функции в методе концентратора, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7c2ed-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="7c2ed-116">Идентификатор пользователя с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a><span data-ttu-id="7c2ed-117">Группами в SignalR</span><span class="sxs-lookup"><span data-stu-id="7c2ed-117">Groups in SignalR</span></span>

<span data-ttu-id="7c2ed-118">Группа — коллекция соединений, связанный с именем.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="7c2ed-119">Сообщения могут отправляться для всех подключений в группе.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="7c2ed-120">Группы — рекомендуемый способ отправить подключения или несколько подключений, так как группы управляются приложением.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="7c2ed-121">Соединение может быть членом нескольких групп.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="7c2ed-122">Это делает групп идеальной для примерно приложения чата, где каждой комнате, можно представить в виде группы.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="7c2ed-123">Соединения можно добавлять к или удалены из групп с помощью `AddToGroupAsync` и `RemoveFromGroupAsync` методы.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="7c2ed-124">Членство в группе не сохраняется при повторном подключении.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="7c2ed-125">Подключение должно повторно присоединиться к группе, когда она будет восстановлена.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="7c2ed-126">Число членов группы, так как эта информация недоступна, если приложение масштабируется на несколько серверов невозможна.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="7c2ed-127">Чтобы защитить доступ к ресурсам при использовании групп, используйте [проверки подлинности и авторизации](xref:signalr/authn-and-authz) функциональные возможности в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="7c2ed-128">Если только добавление пользователей в группу при учетные данные действительны для этой группы сообщений, отправляемых в эту группу только перейдет авторизованным пользователям.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="7c2ed-129">Однако группы не являются функцией безопасности.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-129">However, groups are not a security feature.</span></span> <span data-ttu-id="7c2ed-130">Проверка подлинности утверждений содержат функции, которые группы этого не сделать, например окончания срока действия и отзыв прав доступа.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="7c2ed-131">Если отменяется разрешение пользователя для доступа к группе, необходимо вручную обнаружит это и удалить их из группы.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="7c2ed-132">Имена групп учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="7c2ed-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="7c2ed-133">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7c2ed-133">Related resources</span></span>

* [<span data-ttu-id="7c2ed-134">Начало работы</span><span class="sxs-lookup"><span data-stu-id="7c2ed-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="7c2ed-135">Центры</span><span class="sxs-lookup"><span data-stu-id="7c2ed-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7c2ed-136">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="7c2ed-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
