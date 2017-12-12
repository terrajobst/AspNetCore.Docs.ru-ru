---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "Сопоставление пользователей SignalR с подключениями в SignalR 1.x | Документы Microsoft"
author: pfletcher
description: "В этом разделе показано, как сохранить сведения о пользователях и их соединения."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 561c5739c4e8465efeb4b5d1eaf8a196dab8673f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="b7c00-103">Сопоставление пользователей SignalR с подключениями в SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="b7c00-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="b7c00-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b7c00-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b7c00-105">В этом разделе показано, как сохранить сведения о пользователях и их соединения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="b7c00-106">Вступление</span><span class="sxs-lookup"><span data-stu-id="b7c00-106">Introduction</span></span>

<span data-ttu-id="b7c00-107">Каждый клиент, подключающийся к концентратору передает уникальный идентификатор соединения. Можно получить это значение в `Context.ConnectionId` свойства контекста концентратора.</span><span class="sxs-lookup"><span data-stu-id="b7c00-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="b7c00-108">Если приложению необходимо сопоставить пользователя с идентификатором соединения и сохранить это сопоставление, можно использовать один из следующих:</span><span class="sxs-lookup"><span data-stu-id="b7c00-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="b7c00-109">[Хранение в памяти](#inmemory), такие как словарь</span><span class="sxs-lookup"><span data-stu-id="b7c00-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="b7c00-110">Группа SignalR для каждого пользователя</span><span class="sxs-lookup"><span data-stu-id="b7c00-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="b7c00-111">[Постоянная, внешнего хранилища](#database), таких как базы данных или хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="b7c00-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="b7c00-112">В этом разделе показана каждая из этих реализаций.</span><span class="sxs-lookup"><span data-stu-id="b7c00-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="b7c00-113">Вы используете `OnConnected`, `OnDisconnected`, и `OnReconnected` методы `Hub` класса для отслеживания состояния подключения пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c00-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="b7c00-114">Зависит от оптимального подхода для приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="b7c00-115">Число веб-серверов размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="b7c00-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="b7c00-116">Нужно ли получить список подключенных пользователей.</span><span class="sxs-lookup"><span data-stu-id="b7c00-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="b7c00-117">Следует ли сохранять сведения пользователей и групп, когда приложение или сервер перезагружается.</span><span class="sxs-lookup"><span data-stu-id="b7c00-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="b7c00-118">Задержка при вызове внешнего сервера является ли проблема.</span><span class="sxs-lookup"><span data-stu-id="b7c00-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="b7c00-119">В следующей таблице показано, какой подход работает для этих рекомендациях.</span><span class="sxs-lookup"><span data-stu-id="b7c00-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="b7c00-120">Более одного сервера</span><span class="sxs-lookup"><span data-stu-id="b7c00-120">More than one server</span></span> | <span data-ttu-id="b7c00-121">Получить список подключенных пользователей</span><span class="sxs-lookup"><span data-stu-id="b7c00-121">Get list of currently connected users</span></span> | <span data-ttu-id="b7c00-122">Сохранять данные после перезагрузки</span><span class="sxs-lookup"><span data-stu-id="b7c00-122">Persist information after restarts</span></span> | <span data-ttu-id="b7c00-123">Оптимальной производительности</span><span class="sxs-lookup"><span data-stu-id="b7c00-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="b7c00-124">In-memory</span><span class="sxs-lookup"><span data-stu-id="b7c00-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="b7c00-125">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="b7c00-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="b7c00-126">Постоянными, внешний</span><span class="sxs-lookup"><span data-stu-id="b7c00-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="b7c00-127">Хранение в памяти</span><span class="sxs-lookup"><span data-stu-id="b7c00-127">In-memory storage</span></span>

<span data-ttu-id="b7c00-128">Следующие примеры показывают, как сохранить сведения о соединении и пользователя в словарь, который хранится в памяти.</span><span class="sxs-lookup"><span data-stu-id="b7c00-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="b7c00-129">Словарь использует `HashSet` для хранения идентификатора соединения. В любой момент пользователь может иметь несколько подключений к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="b7c00-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="b7c00-130">Например пользователь, который подключается с помощью нескольких устройств или несколько вкладок браузер будет иметь более одного идентификатор подключения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="b7c00-131">Если приложение завершает работу, все данные теряются, но он будет повторно заполнен как пользователи повторно устанавливать их соединения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="b7c00-132">Хранение в памяти не работает, если в среде имеются несколько веб-сервера, поскольку каждый сервер в отдельную коллекцию соединений.</span><span class="sxs-lookup"><span data-stu-id="b7c00-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="b7c00-133">В первом примере класс, который управляет сопоставления пользователей для подключения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="b7c00-134">Ключ для HashSet будет имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c00-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="b7c00-135">Следующий пример показано, как использовать класс сопоставления подключения от концентратора.</span><span class="sxs-lookup"><span data-stu-id="b7c00-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="b7c00-136">Экземпляр класса хранится в имени переменной `_connections`.</span><span class="sxs-lookup"><span data-stu-id="b7c00-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="b7c00-137">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="b7c00-137">Single-user groups</span></span>

<span data-ttu-id="b7c00-138">Можно создать группу для каждого пользователя и отправить сообщение в эту группу для достижения только данный пользователь.</span><span class="sxs-lookup"><span data-stu-id="b7c00-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="b7c00-139">Имя каждой группы является имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c00-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="b7c00-140">Если пользователь имеет более одного соединения, каждый идентификатор соединения добавляется к группе пользователей.</span><span class="sxs-lookup"><span data-stu-id="b7c00-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="b7c00-141">Не следует удалять вручную пользователя из группы при отключении пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c00-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="b7c00-142">Это действие выполняется автоматически платформой SignalR.</span><span class="sxs-lookup"><span data-stu-id="b7c00-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="b7c00-143">Приведенный ниже показано, как реализовать группы одного пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c00-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="b7c00-144">Постоянные, внешние хранилища</span><span class="sxs-lookup"><span data-stu-id="b7c00-144">Permanent, external storage</span></span>

<span data-ttu-id="b7c00-145">В этом разделе показано, как использовать базы данных или хранилище таблиц Azure для хранения сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="b7c00-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="b7c00-146">Этот способ подходит при наличии нескольких веб-серверов, так как каждый веб-сервер может взаимодействовать с одного репозитория данных.</span><span class="sxs-lookup"><span data-stu-id="b7c00-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="b7c00-147">Если остановить веб-серверов, работает или повторного запуска приложения `OnDisconnected` метод не вызывается.</span><span class="sxs-lookup"><span data-stu-id="b7c00-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="b7c00-148">Таким образом возможно, репозиторий данных будет записи идентификаторов подключений, которые больше не являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="b7c00-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="b7c00-149">Чтобы удалить эти потерянные записи, вы можете сделать недействительной любые соединения, который был создан за пределами периодичностью, необходимые для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="b7c00-150">Примеры в этом разделе содержат значение для отслеживания, когда подключение было создано, но не показано, как удалить старые записи, поскольку вы можете сделать это в фоновом процессе.</span><span class="sxs-lookup"><span data-stu-id="b7c00-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="b7c00-151">База данных</span><span class="sxs-lookup"><span data-stu-id="b7c00-151">Database</span></span>

<span data-ttu-id="b7c00-152">Следующие примеры показывают, как сохранить сведения о соединении и пользователя в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b7c00-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="b7c00-153">Можно использовать любой технологии доступа к данным; Тем не менее в приведенном ниже примере показан способ определения модели с использованием платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7c00-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="b7c00-154">Эти модели сущностей соответствующие таблицы базы данных и полей.</span><span class="sxs-lookup"><span data-stu-id="b7c00-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="b7c00-155">Структуру данных может значительно различаться в зависимости от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="b7c00-156">В первом примере показан способ определения Пользовательская сущность, которая может быть связан с большого количества объектов подключения.</span><span class="sxs-lookup"><span data-stu-id="b7c00-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="b7c00-157">Затем в центре можно отслеживать состояние каждого соединения с приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="b7c00-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="b7c00-158">Хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="b7c00-158">Azure table storage</span></span>

<span data-ttu-id="b7c00-159">В следующем примере хранилища Azure таблицы как в примере базы данных.</span><span class="sxs-lookup"><span data-stu-id="b7c00-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="b7c00-160">Он не включает все данные, что необходимо начать работу со службой хранилища Azure таблицы.</span><span class="sxs-lookup"><span data-stu-id="b7c00-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="b7c00-161">Сведения см. в разделе [как использовать хранилище таблиц из .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="b7c00-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="b7c00-162">В следующем примере показано сущности таблицы для хранения сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="b7c00-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="b7c00-163">Секционирование данных по имени пользователя и идентифицирует каждую сущность, идентификатор подключения, пользователь может иметь несколько подключений в любое время.</span><span class="sxs-lookup"><span data-stu-id="b7c00-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="b7c00-164">В концентраторе отслеживать состояние соединения каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="b7c00-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
