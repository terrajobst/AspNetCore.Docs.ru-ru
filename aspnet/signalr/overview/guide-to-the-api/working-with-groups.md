---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Работа с группами в SignalR | Документы Microsoft
author: pfletcher
description: Описывается, как сохранить сведения о членстве в группах с помощью API концентратора.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042226"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="d94bd-103">Работа с группами в SignalR</span><span class="sxs-lookup"><span data-stu-id="d94bd-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="d94bd-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d94bd-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d94bd-105">В этом разделе описывается добавление пользователей в группы и сохраняет сведения о членстве в группе.</span><span class="sxs-lookup"><span data-stu-id="d94bd-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d94bd-106">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="d94bd-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="d94bd-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d94bd-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d94bd-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d94bd-108">.NET 4.5</span></span>
> - <span data-ttu-id="d94bd-109">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="d94bd-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d94bd-110">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="d94bd-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="d94bd-111">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d94bd-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d94bd-112">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="d94bd-112">Questions and comments</span></span>
> 
> <span data-ttu-id="d94bd-113">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="d94bd-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d94bd-114">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d94bd-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="d94bd-115">Обзор</span><span class="sxs-lookup"><span data-stu-id="d94bd-115">Overview</span></span>

<span data-ttu-id="d94bd-116">Группы в SignalR предоставляют метод для широковещательной рассылки сообщений для указанного подмножества подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="d94bd-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="d94bd-117">Группа может иметь любое количество клиентов, и клиент может быть членом любое количество групп.</span><span class="sxs-lookup"><span data-stu-id="d94bd-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="d94bd-118">Не нужно явно создавать группы.</span><span class="sxs-lookup"><span data-stu-id="d94bd-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="d94bd-119">В результате группа автоматически создается впервые, укажите его имя в вызове Groups.Add и удаляется при удалении последнего соединения через членство в ней.</span><span class="sxs-lookup"><span data-stu-id="d94bd-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="d94bd-120">Сведения об использовании групп, в разделе [как управлять членством в группах от концентратора класса](hubs-api-guide-server.md#groupsfromhub) в API концентраторов — руководство по Server.</span><span class="sxs-lookup"><span data-stu-id="d94bd-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="d94bd-121">Нет никакой интерфейс API для получения список членства в группе или список групп.</span><span class="sxs-lookup"><span data-stu-id="d94bd-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="d94bd-122">SignalR отправляет сообщения клиентам и группам на основе модели публикации и подписке, а сервер не поддерживает списки групп или членства в группах.</span><span class="sxs-lookup"><span data-stu-id="d94bd-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="d94bd-123">Это позволяет увеличить масштабируемость, так как каждый раз при добавлении узла к веб-ферме, любое состояние, которое поддерживает SignalR для нового узла.</span><span class="sxs-lookup"><span data-stu-id="d94bd-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="d94bd-124">При добавлении пользователя в группу с помощью `Groups.Add` метод, пользователь получает сообщения направляются в эту группу, в течение текущего соединения, но членства пользователя в этой группе не сохраняется вне текущего соединения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="d94bd-125">Если вы хотите окончательно сохранить сведения о группах и членства в группе, эти данные необходимо хранить в хранилище, например базы данных или хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="d94bd-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="d94bd-126">Затем при каждом подключении пользователя к приложению, получения из репозитория каким группам принадлежит пользователь и вручную добавьте пользователя к группам.</span><span class="sxs-lookup"><span data-stu-id="d94bd-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="d94bd-127">При повторном подключении после временному перерыву в предоставлении, пользователь автоматически повторно присоединяет групп ранее назначены.</span><span class="sxs-lookup"><span data-stu-id="d94bd-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="d94bd-128">Автоматическое повторное присоединение группы применяется только при повторном подключении не при установке нового подключения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="d94bd-129">Токен цифровой подписью передается от клиента, который содержит список групп ранее назначены.</span><span class="sxs-lookup"><span data-stu-id="d94bd-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="d94bd-130">Если вы хотите проверить принадлежность пользователя к запрошенной группы, можно переопределить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d94bd-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="d94bd-131">Этот раздел включает следующие подразделы:</span><span class="sxs-lookup"><span data-stu-id="d94bd-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="d94bd-132">Добавление и удаление пользователей</span><span class="sxs-lookup"><span data-stu-id="d94bd-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="d94bd-133">Вызов членов группы</span><span class="sxs-lookup"><span data-stu-id="d94bd-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="d94bd-134">Хранение в базе данных членства в группе</span><span class="sxs-lookup"><span data-stu-id="d94bd-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="d94bd-135">Членство в группе хранения в хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="d94bd-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="d94bd-136">Проверка членства в группе при повторном подключении</span><span class="sxs-lookup"><span data-stu-id="d94bd-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="d94bd-137">Добавление и удаление пользователей</span><span class="sxs-lookup"><span data-stu-id="d94bd-137">Adding and removing users</span></span>

<span data-ttu-id="d94bd-138">Чтобы добавить или удалить пользователей из группы, следует вызвать [добавить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) или [удалить](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) методы и передайте ему идентификатор соединения пользователя и имя группы в качестве параметров.</span><span class="sxs-lookup"><span data-stu-id="d94bd-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="d94bd-139">Необходимо вручную удалить пользователя из группы при завершении соединения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="d94bd-140">В следующем примере показан `Groups.Add` и `Groups.Remove` методы, которые используются в методах Hub.</span><span class="sxs-lookup"><span data-stu-id="d94bd-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="d94bd-141">`Groups.Add` И `Groups.Remove` методы асинхронного выполнения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="d94bd-142">Если вы хотите добавить в группу клиента и немедленно отправить клиенту сообщение с помощью в группу необходимо убедитесь в том, что метод Groups.Add сначала завершается.</span><span class="sxs-lookup"><span data-stu-id="d94bd-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="d94bd-143">В следующем примере кода показано, как сделать это.</span><span class="sxs-lookup"><span data-stu-id="d94bd-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="d94bd-144">В общем случае не следует включать `await` при вызове `Groups.Remove` метода, так как идентификатор соединения, который вы пытаетесь удалить становятся доступны.</span><span class="sxs-lookup"><span data-stu-id="d94bd-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="d94bd-145">В этом случае `TaskCanceledException` возникает после истечения срока действия запроса. Если приложение необходимо убедиться, что пользователь был удален из группы перед отправкой сообщения в группу, можно добавить `await` до Groups.Remove, а затем catch `TaskCanceledException` исключение, которое может быть создано.</span><span class="sxs-lookup"><span data-stu-id="d94bd-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="d94bd-146">Вызов членов группы</span><span class="sxs-lookup"><span data-stu-id="d94bd-146">Calling members of a group</span></span>

<span data-ttu-id="d94bd-147">Можно отправлять сообщения для всех членов группы или только указанные члены группы, как показано в следующих примерах.</span><span class="sxs-lookup"><span data-stu-id="d94bd-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="d94bd-148">**Все** подключенных клиентов в указанной группе.</span><span class="sxs-lookup"><span data-stu-id="d94bd-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="d94bd-149">Все подключенные клиенты в указанной группе **указанным клиентам, за исключением**, указанный с помощью идентификатор соединения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="d94bd-150">Все подключенные клиенты в указанной группе **, кроме вызывающего клиента**.</span><span class="sxs-lookup"><span data-stu-id="d94bd-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="d94bd-151">Хранение в базе данных членства в группе</span><span class="sxs-lookup"><span data-stu-id="d94bd-151">Storing group membership in a database</span></span>

<span data-ttu-id="d94bd-152">В следующих примерах для сохранения данных пользователей и групп в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d94bd-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="d94bd-153">Можно использовать любой технологии доступа к данным; Тем не менее в приведенном ниже примере показан способ определения модели с использованием платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d94bd-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="d94bd-154">Эти модели сущностей соответствующие таблицы базы данных и полей.</span><span class="sxs-lookup"><span data-stu-id="d94bd-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="d94bd-155">Структуру данных может значительно различаться в зависимости от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="d94bd-156">В этом примере включает класс `ConversationRoom` которого будут уникальны для приложения, которое позволяет пользователям присоединяться к обсуждения различных задач, таких как спортивных или садом.</span><span class="sxs-lookup"><span data-stu-id="d94bd-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="d94bd-157">Этот пример также содержит класс для соединения.</span><span class="sxs-lookup"><span data-stu-id="d94bd-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="d94bd-158">Класс соединений не являются необходимыми для отслеживания членство в группе, но часто является частью надежное решение для отслеживания пользователей.</span><span class="sxs-lookup"><span data-stu-id="d94bd-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="d94bd-159">Затем в концентраторе, можно получить сведения о группе и пользователя из базы данных и вручную добавьте пользователя в соответствующие группы.</span><span class="sxs-lookup"><span data-stu-id="d94bd-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="d94bd-160">Пример не включает код для отслеживания пользовательских соединений.</span><span class="sxs-lookup"><span data-stu-id="d94bd-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="d94bd-161">В этом примере `await` ключевое слово не применяется перед `Groups.Add` так, как сообщения не отправляются немедленно для членов группы.</span><span class="sxs-lookup"><span data-stu-id="d94bd-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="d94bd-162">Если вы хотите отправить сообщение всем членам группы сразу после добавления нового члена, может потребоваться применить `await` ключевое слово, чтобы убедиться, что асинхронная операция завершена.</span><span class="sxs-lookup"><span data-stu-id="d94bd-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="d94bd-163">Членство в группе хранения в хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="d94bd-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="d94bd-164">С помощью табличного хранилища Azure для хранения данных пользователей и групп аналогична использованию базы данных.</span><span class="sxs-lookup"><span data-stu-id="d94bd-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="d94bd-165">В следующем примере показано сущности таблицы, в которой хранится имя пользователя и имя группы.</span><span class="sxs-lookup"><span data-stu-id="d94bd-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="d94bd-166">В концентраторе получить назначенной группы при подключении.</span><span class="sxs-lookup"><span data-stu-id="d94bd-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="d94bd-167">Проверка членства в группе при повторном подключении</span><span class="sxs-lookup"><span data-stu-id="d94bd-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="d94bd-168">По умолчанию SignalR автоматически повторно назначает пользователя в соответствующие группы при повторном подключении от временного прерывания, таких как после удалены и повторно установить подключение времени ожидания соединения. Сведения о группе пользователя передается в токене при повторном подключении, и этот маркер проверяется на сервере.</span><span class="sxs-lookup"><span data-stu-id="d94bd-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="d94bd-169">Сведения о процессе проверки повторное присоединение пользователей к группам см. в разделе [повторное присоединение групп при повторном подключении](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="d94bd-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="d94bd-170">В общем случае следует использовать поведение по умолчанию автоматически присоединяется что групп на повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="d94bd-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="d94bd-171">SignalR группы не войдут в качестве механизма безопасности для ограничения доступа к конфиденциальным данным.</span><span class="sxs-lookup"><span data-stu-id="d94bd-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="d94bd-172">Тем не менее если приложение необходимо тщательно членства пользователя в группе при повторном подключении, можно переопределить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d94bd-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="d94bd-173">Изменение поведения по умолчанию можно добавить нагрузку для базы данных, так как членство в группе пользователей должны извлекаться для каждого повторное подключение, а не только в том случае, когда пользователь подключается.</span><span class="sxs-lookup"><span data-stu-id="d94bd-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="d94bd-174">Если необходимо проверить членство в группе на повторное подключение, создайте новый модуль конвейер концентратора, возвращающий список назначенных групп, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="d94bd-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="d94bd-175">Затем добавьте этот модуль в конвейер концентратора, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="d94bd-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
