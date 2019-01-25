---
uid: signalr/overview/older-versions/working-with-groups
title: Работа с группами в SignalR 1.x | Документация Майкрософт
author: bradygaster
description: В этом разделе описывается, как сохранять данные членства в группе с помощью API концентратора.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: e8ef7b34af4341fb97f54e2aab1d8971cce46af3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836159"
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="04e1c-103">Работа с группами в SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="04e1c-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="04e1c-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="04e1c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="04e1c-105">В этом разделе описывается добавление пользователей в группы и сохранять данные членства в группе.</span><span class="sxs-lookup"><span data-stu-id="04e1c-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="04e1c-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="04e1c-106">Overview</span></span>

<span data-ttu-id="04e1c-107">Группами в SignalR предоставляют метод широковещательная рассылка сообщений для заданного подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="04e1c-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="04e1c-108">Группа может иметь любое число клиентов, и клиент может быть членом любое количество групп.</span><span class="sxs-lookup"><span data-stu-id="04e1c-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="04e1c-109">Не нужно явно создавать группы.</span><span class="sxs-lookup"><span data-stu-id="04e1c-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="04e1c-110">По сути группа автоматически создается первый раз, укажите его имя в вызове Groups.Add, и она будет удалена при удалении последнего соединения из членства в ней.</span><span class="sxs-lookup"><span data-stu-id="04e1c-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="04e1c-111">Введение в использование групп, см. в разделе [как управлять членством в группах из классу Hub](index.md) в API концентраторов — руководство по Server.</span><span class="sxs-lookup"><span data-stu-id="04e1c-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="04e1c-112">Не существует API для получения списка членства в группе или список групп.</span><span class="sxs-lookup"><span data-stu-id="04e1c-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="04e1c-113">SignalR отправляет сообщения клиентов и группы на основе модели публикации и подписки, а сервер не ведет список групп или членства в группах.</span><span class="sxs-lookup"><span data-stu-id="04e1c-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="04e1c-114">Это помогает добиться максимальной масштабируемости, так как каждый раз при добавлении узла к веб-ферме, любое состояние, которое поддерживает SignalR должен быть распространены на новый узел.</span><span class="sxs-lookup"><span data-stu-id="04e1c-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="04e1c-115">При добавлении пользователя в группу с помощью `Groups.Add` метод, пользователь получает сообщения, направленных в эту группу, в течение текущего соединения, но членства пользователя в этой группе не сохраняется вне текущего соединения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="04e1c-116">Если вы хотите окончательно сохранить сведения о группах и членства в группе, необходимо хранить их в репозитории, например базы данных или хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="04e1c-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="04e1c-117">Затем каждый раз при подключении пользователя к приложению, извлечь из хранилища, каким группам принадлежит пользователь и вручную добавить этого пользователя к группам.</span><span class="sxs-lookup"><span data-stu-id="04e1c-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="04e1c-118">При повторном подключении после к временному нарушению работы, пользователь автоматически повторно присоединяет ранее назначенные группы.</span><span class="sxs-lookup"><span data-stu-id="04e1c-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="04e1c-119">Автоматическое повторное присоединение группы применяется только при повторном подключении не в том случае, при установке нового подключения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="04e1c-120">Токен цифровую подпись передается от клиента, который содержит список ранее назначенные группы.</span><span class="sxs-lookup"><span data-stu-id="04e1c-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="04e1c-121">Если вы хотите проверить, принадлежит ли пользователь в запрошенной группы, можно переопределить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="04e1c-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="04e1c-122">Этот раздел включает следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="04e1c-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="04e1c-123">Добавление и удаление пользователей</span><span class="sxs-lookup"><span data-stu-id="04e1c-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="04e1c-124">Вызов членов группы</span><span class="sxs-lookup"><span data-stu-id="04e1c-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="04e1c-125">Хранение в базе данных членства в группе</span><span class="sxs-lookup"><span data-stu-id="04e1c-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="04e1c-126">Сохранение членства в группе в хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="04e1c-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="04e1c-127">Проверка членства в группе, при повторном подключении</span><span class="sxs-lookup"><span data-stu-id="04e1c-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="04e1c-128">Добавление и удаление пользователей</span><span class="sxs-lookup"><span data-stu-id="04e1c-128">Adding and removing users</span></span>

<span data-ttu-id="04e1c-129">Чтобы добавить или удалить пользователей из группы, следует вызвать [добавить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) или [удалить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) методы и передайте идентификатор соединения пользователя и имя группы в качестве параметров.</span><span class="sxs-lookup"><span data-stu-id="04e1c-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="04e1c-130">Необходимо вручную удалить пользователя из группы, по окончании соединения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="04e1c-131">В следующем примере показан `Groups.Add` и `Groups.Remove` методы, используемые в методах Hub.</span><span class="sxs-lookup"><span data-stu-id="04e1c-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="04e1c-132">`Groups.Add` И `Groups.Remove` методы асинхронного выполнения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="04e1c-133">Если вы хотите добавить в клиент группу и немедленно отправить клиенту сообщение с помощью в группу, необходимо убедитесь в том, что метод Groups.Add завершается первой.</span><span class="sxs-lookup"><span data-stu-id="04e1c-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="04e1c-134">В следующих примерах кода показано, как это сделать с помощью кода, который работает в .NET 4.5, а другой с помощью кода, который работает в .NET 4.</span><span class="sxs-lookup"><span data-stu-id="04e1c-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="04e1c-135">Асинхронные .NET 4.5 пример</span><span class="sxs-lookup"><span data-stu-id="04e1c-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="04e1c-136">Асинхронные .NET 4 пример</span><span class="sxs-lookup"><span data-stu-id="04e1c-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="04e1c-137">В общем случае не следует включать `await` при вызове `Groups.Remove` метод так как идентификатор подключения, который вы пытаетесь удалить больше не могут быть доступны.</span><span class="sxs-lookup"><span data-stu-id="04e1c-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="04e1c-138">В этом случае `TaskCanceledException` возникает после тайм-аута запроса. Если приложение должно убедиться, что пользователь удален из группы перед отправкой сообщения в группу, можно добавить `await` перед Groups.Remove, а затем catch `TaskCanceledException` исключение, которое может быть создано.</span><span class="sxs-lookup"><span data-stu-id="04e1c-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="04e1c-139">Вызов членов группы</span><span class="sxs-lookup"><span data-stu-id="04e1c-139">Calling members of a group</span></span>

<span data-ttu-id="04e1c-140">Можно отправлять сообщения для всех участников группы или только указанные члены группы, как показано в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="04e1c-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="04e1c-141">**Все** подключенных клиентов в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="04e1c-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="04e1c-142">Все подключенные клиенты в указанной группе **указанным клиентам, за исключением**, идентифицируемый идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="04e1c-143">Все подключенные клиенты в указанной группе **, кроме вызывающего клиента**.</span><span class="sxs-lookup"><span data-stu-id="04e1c-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="04e1c-144">Хранение в базе данных членства в группе</span><span class="sxs-lookup"><span data-stu-id="04e1c-144">Storing group membership in a database</span></span>

<span data-ttu-id="04e1c-145">Следующие примеры показывают, как сохранять сведения о пользователей и групп в базе данных.</span><span class="sxs-lookup"><span data-stu-id="04e1c-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="04e1c-146">Можно использовать любые технологии доступа к данным; Тем не менее в приведенном ниже примере показано, как для определения моделей с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="04e1c-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="04e1c-147">Эти модели сущности соответствуют таблиц базы данных и полей.</span><span class="sxs-lookup"><span data-stu-id="04e1c-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="04e1c-148">Структуру данных может значительно изменяться в зависимости от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="04e1c-149">Этот пример включает класс с именем `ConversationRoom` которой бы быть уникальными в приложение, которое позволяет пользователям присоединяться к беседы о различных задач, таких как спорта и садоводство.</span><span class="sxs-lookup"><span data-stu-id="04e1c-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="04e1c-150">Этот пример также содержит класс для соединения.</span><span class="sxs-lookup"><span data-stu-id="04e1c-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="04e1c-151">Класс подключения не являются необходимыми для отслеживания членство в группе, но часто является частью надежное решение для отслеживания пользователей.</span><span class="sxs-lookup"><span data-stu-id="04e1c-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="04e1c-152">Затем в концентраторе, можно извлечь данные группы и пользователя из базы данных и вручную добавить пользователя в соответствующие группы.</span><span class="sxs-lookup"><span data-stu-id="04e1c-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="04e1c-153">Пример не содержит код для отслеживания пользовательских соединений.</span><span class="sxs-lookup"><span data-stu-id="04e1c-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="04e1c-154">В этом примере `await` ключевое слово не применяется перед `Groups.Add` потому, что сообщение не отправляется сразу же членами группы.</span><span class="sxs-lookup"><span data-stu-id="04e1c-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="04e1c-155">Если вы хотите отправить сообщение всем членам группы сразу после добавления нового члена, можно применить `await` ключевое слово, чтобы убедиться, что асинхронная операция завершена.</span><span class="sxs-lookup"><span data-stu-id="04e1c-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="04e1c-156">Сохранение членства в группе в хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="04e1c-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="04e1c-157">Использование табличного хранилища Azure для хранения информации, пользователей и групп похоже на использование базы данных.</span><span class="sxs-lookup"><span data-stu-id="04e1c-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="04e1c-158">В следующем примере показано сущности таблицы, в котором хранится имя пользователя и имя группы.</span><span class="sxs-lookup"><span data-stu-id="04e1c-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="04e1c-159">В концентраторе вы получите назначенных групп при подключении.</span><span class="sxs-lookup"><span data-stu-id="04e1c-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="04e1c-160">Проверка членства в группе, при повторном подключении</span><span class="sxs-lookup"><span data-stu-id="04e1c-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="04e1c-161">По умолчанию SignalR автоматически повторно назначает пользователя в соответствующие группы при повторном подключении из к временному нарушению работы, например при удалении и заново установить соединение, время ожидания соединения. Сведения о группе пользователя передается в токене при повторном подключении, и этот маркер проверяется на сервере.</span><span class="sxs-lookup"><span data-stu-id="04e1c-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="04e1c-162">Сведения о процессе проверки для повторное присоединение пользователей к группам, см. в разделе [повторное присоединение групп при повторном подключении](index.md).</span><span class="sxs-lookup"><span data-stu-id="04e1c-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="04e1c-163">В общем случае следует использовать поведение по умолчанию автоматически повторное присоединение что групп на повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="04e1c-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="04e1c-164">SignalR группы не следует рассматривать как механизм безопасности для ограничения доступа к конфиденциальным данным.</span><span class="sxs-lookup"><span data-stu-id="04e1c-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="04e1c-165">Тем не менее если приложение необходимо тщательно проверить членство в группе пользователей, при повторном подключении, можно переопределить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="04e1c-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="04e1c-166">Изменение поведения по умолчанию можно добавить и нагрузку в базу данных так, как членство в группе пользователей, которые должны быть получены, для каждого повторного подключения, а не только в том случае, когда пользователь подключается.</span><span class="sxs-lookup"><span data-stu-id="04e1c-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="04e1c-167">Если вам необходимо проверить членство в группе на повторное подключение, создание нового модуля конвейер концентратора, который возвращает список всех назначенных групп, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="04e1c-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="04e1c-168">Затем добавьте этот модуль в конвейер концентратора, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="04e1c-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
