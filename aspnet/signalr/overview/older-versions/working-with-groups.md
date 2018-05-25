---
uid: signalr/overview/older-versions/working-with-groups
title: Работа с группами в SignalR 1.x | Документы Microsoft
author: pfletcher
description: Описывается, как сохранить сведения о членстве в группах с помощью API концентратора.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="25b2e-103">Работа с группами в SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="25b2e-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="25b2e-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="25b2e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="25b2e-105">В этом разделе описывается добавление пользователей в группы и сохраняет сведения о членстве в группе.</span><span class="sxs-lookup"><span data-stu-id="25b2e-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="25b2e-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="25b2e-106">Overview</span></span>

<span data-ttu-id="25b2e-107">Группы в SignalR предоставляют метод для широковещательной рассылки сообщений для указанного подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="25b2e-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="25b2e-108">Группа может иметь любое количество клиентов, и клиент может быть членом любое количество групп.</span><span class="sxs-lookup"><span data-stu-id="25b2e-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="25b2e-109">Не нужно явно создавать группы.</span><span class="sxs-lookup"><span data-stu-id="25b2e-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="25b2e-110">В результате группа автоматически создается впервые, укажите его имя в вызове Groups.Add и удаляется при удалении последнего соединения через членство в ней.</span><span class="sxs-lookup"><span data-stu-id="25b2e-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="25b2e-111">Сведения об использовании групп, в разделе [как управлять членством в группах от концентратора класса](index.md) в API концентраторов — руководство по Server.</span><span class="sxs-lookup"><span data-stu-id="25b2e-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="25b2e-112">Нет никакой интерфейс API для получения список членства в группе или список групп.</span><span class="sxs-lookup"><span data-stu-id="25b2e-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="25b2e-113">SignalR отправляет сообщения клиентам и группам на основе модели публикации и подписке, а сервер не поддерживает списки групп или членства в группах.</span><span class="sxs-lookup"><span data-stu-id="25b2e-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="25b2e-114">Это позволяет увеличить масштабируемость, так как каждый раз при добавлении узла к веб-ферме, любое состояние, которое поддерживает SignalR для нового узла.</span><span class="sxs-lookup"><span data-stu-id="25b2e-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="25b2e-115">При добавлении пользователя в группу с помощью `Groups.Add` метод, пользователь получает сообщения направляются в эту группу, в течение текущего соединения, но членства пользователя в этой группе не сохраняется вне текущего соединения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="25b2e-116">Если вы хотите окончательно сохранить сведения о группах и членства в группе, эти данные необходимо хранить в хранилище, например базы данных или хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="25b2e-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="25b2e-117">Затем при каждом подключении пользователя к приложению, получения из репозитория каким группам принадлежит пользователь и вручную добавьте пользователя к группам.</span><span class="sxs-lookup"><span data-stu-id="25b2e-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="25b2e-118">При повторном подключении после временному перерыву в предоставлении, пользователь автоматически повторно присоединяет групп ранее назначены.</span><span class="sxs-lookup"><span data-stu-id="25b2e-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="25b2e-119">Автоматическое повторное присоединение группы применяется только при повторном подключении не при установке нового подключения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="25b2e-120">Токен цифровой подписью передается от клиента, который содержит список групп ранее назначены.</span><span class="sxs-lookup"><span data-stu-id="25b2e-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="25b2e-121">Если вы хотите проверить принадлежность пользователя к запрошенной группы, можно переопределить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="25b2e-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="25b2e-122">Этот раздел включает следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="25b2e-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="25b2e-123">Добавление и удаление пользователей</span><span class="sxs-lookup"><span data-stu-id="25b2e-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="25b2e-124">Вызов членов группы</span><span class="sxs-lookup"><span data-stu-id="25b2e-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="25b2e-125">Хранение в базе данных членства в группе</span><span class="sxs-lookup"><span data-stu-id="25b2e-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="25b2e-126">Членство в группе хранения в хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="25b2e-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="25b2e-127">Проверка членства в группе при повторном подключении</span><span class="sxs-lookup"><span data-stu-id="25b2e-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="25b2e-128">Добавление и удаление пользователей</span><span class="sxs-lookup"><span data-stu-id="25b2e-128">Adding and removing users</span></span>

<span data-ttu-id="25b2e-129">Чтобы добавить или удалить пользователей из группы, следует вызвать [добавить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) или [удалить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) методы и передайте ему идентификатор соединения пользователя и имя группы в качестве параметров.</span><span class="sxs-lookup"><span data-stu-id="25b2e-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="25b2e-130">Необходимо вручную удалить пользователя из группы при завершении соединения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="25b2e-131">В следующем примере показан `Groups.Add` и `Groups.Remove` методы, которые используются в методах Hub.</span><span class="sxs-lookup"><span data-stu-id="25b2e-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="25b2e-132">`Groups.Add` И `Groups.Remove` методы асинхронного выполнения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="25b2e-133">Если вы хотите добавить в группу клиента и немедленно отправить клиенту сообщение с помощью в группу необходимо убедитесь в том, что метод Groups.Add сначала завершается.</span><span class="sxs-lookup"><span data-stu-id="25b2e-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="25b2e-134">В следующем примере кода показано, как сделать это с помощью кода, который работает в .NET 4.5, а другой с помощью кода, который работает в .NET 4.</span><span class="sxs-lookup"><span data-stu-id="25b2e-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="25b2e-135">Асинхронные .NET 4.5 пример</span><span class="sxs-lookup"><span data-stu-id="25b2e-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="25b2e-136">Асинхронные .NET Framework 4 пример</span><span class="sxs-lookup"><span data-stu-id="25b2e-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="25b2e-137">В общем случае не следует включать `await` при вызове `Groups.Remove` метода, так как идентификатор соединения, который вы пытаетесь удалить становятся доступны.</span><span class="sxs-lookup"><span data-stu-id="25b2e-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="25b2e-138">В этом случае `TaskCanceledException` возникает после истечения срока действия запроса. Если приложение необходимо убедиться, что пользователь был удален из группы перед отправкой сообщения в группу, можно добавить `await` до Groups.Remove, а затем catch `TaskCanceledException` исключение, которое может быть создано.</span><span class="sxs-lookup"><span data-stu-id="25b2e-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="25b2e-139">Вызов членов группы</span><span class="sxs-lookup"><span data-stu-id="25b2e-139">Calling members of a group</span></span>

<span data-ttu-id="25b2e-140">Можно отправлять сообщения для всех членов группы или только указанные члены группы, как показано в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="25b2e-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="25b2e-141">**Все** подключенных клиентов в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="25b2e-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="25b2e-142">Все подключенные клиенты в указанной группе **указанным клиентам, за исключением**, указанный с помощью идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="25b2e-143">Все подключенные клиенты в указанной группе **, кроме вызывающего клиента**.</span><span class="sxs-lookup"><span data-stu-id="25b2e-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="25b2e-144">Хранение в базе данных членства в группе</span><span class="sxs-lookup"><span data-stu-id="25b2e-144">Storing group membership in a database</span></span>

<span data-ttu-id="25b2e-145">В следующих примерах для сохранения данных пользователей и групп в базе данных.</span><span class="sxs-lookup"><span data-stu-id="25b2e-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="25b2e-146">Можно использовать любой технологии доступа к данным; Тем не менее в приведенном ниже примере показан способ определения модели с использованием платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25b2e-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="25b2e-147">Эти модели сущностей соответствующие таблицы базы данных и полей.</span><span class="sxs-lookup"><span data-stu-id="25b2e-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="25b2e-148">Структуру данных может значительно различаться в зависимости от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="25b2e-149">В этом примере включает класс `ConversationRoom` которого будут уникальны для приложения, которое позволяет пользователям присоединяться к обсуждения различных задач, таких как спортивных или садом.</span><span class="sxs-lookup"><span data-stu-id="25b2e-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="25b2e-150">Этот пример также содержит класс для соединения.</span><span class="sxs-lookup"><span data-stu-id="25b2e-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="25b2e-151">Класс соединений не являются необходимыми для отслеживания членство в группе, но часто является частью надежное решение для отслеживания пользователей.</span><span class="sxs-lookup"><span data-stu-id="25b2e-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="25b2e-152">Затем в концентраторе, можно получить сведения о группе и пользователя из базы данных и вручную добавьте пользователя в соответствующие группы.</span><span class="sxs-lookup"><span data-stu-id="25b2e-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="25b2e-153">Пример не включает код для отслеживания пользовательских соединений.</span><span class="sxs-lookup"><span data-stu-id="25b2e-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="25b2e-154">В этом примере `await` ключевое слово не применяется перед `Groups.Add` так, как сообщения не отправляются немедленно для членов группы.</span><span class="sxs-lookup"><span data-stu-id="25b2e-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="25b2e-155">Если вы хотите отправить сообщение всем членам группы сразу после добавления нового члена, может потребоваться применить `await` ключевое слово, чтобы убедиться, что асинхронная операция завершена.</span><span class="sxs-lookup"><span data-stu-id="25b2e-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="25b2e-156">Членство в группе хранения в хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="25b2e-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="25b2e-157">С помощью табличного хранилища Azure для хранения данных пользователей и групп аналогична использованию базы данных.</span><span class="sxs-lookup"><span data-stu-id="25b2e-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="25b2e-158">В следующем примере показано сущности таблицы, в которой хранится имя пользователя и имя группы.</span><span class="sxs-lookup"><span data-stu-id="25b2e-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="25b2e-159">В концентраторе получить назначенной группы при подключении.</span><span class="sxs-lookup"><span data-stu-id="25b2e-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="25b2e-160">Проверка членства в группе при повторном подключении</span><span class="sxs-lookup"><span data-stu-id="25b2e-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="25b2e-161">По умолчанию SignalR автоматически повторно назначает пользователя в соответствующие группы при повторном подключении от временного прерывания, таких как после удалены и повторно установить подключение времени ожидания соединения. Сведения о группе пользователя передается в токене при повторном подключении, и этот маркер проверяется на сервере.</span><span class="sxs-lookup"><span data-stu-id="25b2e-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="25b2e-162">Сведения о процессе проверки повторное присоединение пользователей к группам см. в разделе [повторное присоединение групп при повторном подключении](index.md).</span><span class="sxs-lookup"><span data-stu-id="25b2e-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="25b2e-163">В общем случае следует использовать поведение по умолчанию автоматически присоединяется что групп на повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="25b2e-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="25b2e-164">SignalR группы не войдут в качестве механизма безопасности для ограничения доступа к конфиденциальным данным.</span><span class="sxs-lookup"><span data-stu-id="25b2e-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="25b2e-165">Тем не менее если приложение необходимо тщательно членства пользователя в группе при повторном подключении, можно переопределить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="25b2e-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="25b2e-166">Изменение поведения по умолчанию можно добавить нагрузку для базы данных, так как членство в группе пользователей должны извлекаться для каждого повторное подключение, а не только в том случае, когда пользователь подключается.</span><span class="sxs-lookup"><span data-stu-id="25b2e-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="25b2e-167">Если необходимо проверить членство в группе на повторное подключение, создайте новый модуль конвейер концентратора, возвращающий список назначенных групп, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="25b2e-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="25b2e-168">Затем добавьте этот модуль в конвейер концентратора, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="25b2e-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
