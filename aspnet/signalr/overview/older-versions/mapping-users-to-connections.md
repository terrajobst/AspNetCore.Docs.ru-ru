---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Сопоставление пользователей SignalR с подключениями в SignalR 1.x | Документация Майкрософт
author: pfletcher
description: В этом разделе показано, как сохранить сведения о пользователях и их подключений.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: ba7bbea25cd35f8ba23901c5f8db08404c91bd52
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287492"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="f8b1b-103">Сопоставление пользователей SignalR с подключениями в SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="f8b1b-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="f8b1b-104">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f8b1b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="f8b1b-105">В этом разделе показано, как сохранить сведения о пользователях и их подключений.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="f8b1b-106">Вступление</span><span class="sxs-lookup"><span data-stu-id="f8b1b-106">Introduction</span></span>

<span data-ttu-id="f8b1b-107">Каждый клиент, подключающийся к концентратору передает уникальный идентификатор соединения. Можно получить это значение в `Context.ConnectionId` свойство контекста концентратора.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="f8b1b-108">Если приложению требуется сопоставить пользователя с идентификатором соединения и сохранить это сопоставление, можно использовать один из следующих:</span><span class="sxs-lookup"><span data-stu-id="f8b1b-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="f8b1b-109">[Хранилища in-memory](#inmemory), такие как словарь</span><span class="sxs-lookup"><span data-stu-id="f8b1b-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="f8b1b-110">Группа SignalR для каждого пользователя</span><span class="sxs-lookup"><span data-stu-id="f8b1b-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="f8b1b-111">[Постоянная, внешние хранилища](#database), например таблицы базы данных или хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="f8b1b-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="f8b1b-112">Каждая из этих реализаций будет показано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="f8b1b-113">Использовании `OnConnected`, `OnDisconnected`, и `OnReconnected` методы `Hub` класса для отслеживания состояния подключения пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="f8b1b-114">Зависит от оптимального подхода для приложения:</span><span class="sxs-lookup"><span data-stu-id="f8b1b-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="f8b1b-115">Число веб-серверов, размещение приложения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="f8b1b-116">Нужно ли получить список подключенных пользователей.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="f8b1b-117">Нужно ли сохранять данные пользователей и групп, после перезапуска приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="f8b1b-118">Задержка при вызове внешнего сервера, является ли проблема.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="f8b1b-119">В следующей таблице показано, какой метод подходит для этих факторов.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="f8b1b-120">Более одного сервера</span><span class="sxs-lookup"><span data-stu-id="f8b1b-120">More than one server</span></span> | <span data-ttu-id="f8b1b-121">Получить список подключенных пользователей</span><span class="sxs-lookup"><span data-stu-id="f8b1b-121">Get list of currently connected users</span></span> | <span data-ttu-id="f8b1b-122">Сохранять данные после перезагрузки</span><span class="sxs-lookup"><span data-stu-id="f8b1b-122">Persist information after restarts</span></span> | <span data-ttu-id="f8b1b-123">Оптимальной производительности</span><span class="sxs-lookup"><span data-stu-id="f8b1b-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="f8b1b-124">In-memory</span><span class="sxs-lookup"><span data-stu-id="f8b1b-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="f8b1b-125">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="f8b1b-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="f8b1b-126">Постоянные, внешние</span><span class="sxs-lookup"><span data-stu-id="f8b1b-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="f8b1b-127">Хранилище в памяти</span><span class="sxs-lookup"><span data-stu-id="f8b1b-127">In-memory storage</span></span>

<span data-ttu-id="f8b1b-128">Следующие примеры показывают, как сохранить сведения о соединении и пользователя в словаре, который хранится в памяти.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="f8b1b-129">Словарь использует `HashSet` для хранения идентификатор подключения. В любой момент пользователь может иметь несколько подключений к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="f8b1b-130">Например пользователь, подключенный через несколько устройств или несколько вкладок браузера будет иметь более одного идентификатор подключения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="f8b1b-131">Если приложение завершает работу, все данные теряются, но он будет повторно заполнен как пользователям восстановить свои соединения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="f8b1b-132">Хранилище в памяти не работает, если в среде имеются несколько веб-сервер, поскольку каждый сервер в отдельной коллекции подключений.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="f8b1b-133">В первом примере класс, который управляет сопоставление пользователей для подключения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="f8b1b-134">Ключ для класса HashSet будет имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="f8b1b-135">Далее примере показано, как использовать класс сопоставление подключения с помощью центра.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="f8b1b-136">Экземпляр класса хранится в переменной с именем `_connections`.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="f8b1b-137">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="f8b1b-137">Single-user groups</span></span>

<span data-ttu-id="f8b1b-138">Можно создать группу для каждого пользователя и отправить сообщение в эту группу, если вы хотите связаться только этот пользователь.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="f8b1b-139">Имя каждой группы — это имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="f8b1b-140">Если у пользователя есть больше одного соединения, каждый идентификатор соединения добавляется к группе пользователей.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="f8b1b-141">Не следует удалять вручную пользователь из группы при отключении пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="f8b1b-142">Это действие автоматически выполняется платформой SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="f8b1b-143">Приведенный ниже показано, как реализовывать группы одного пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="f8b1b-144">Постоянная, внешнего хранилища</span><span class="sxs-lookup"><span data-stu-id="f8b1b-144">Permanent, external storage</span></span>

<span data-ttu-id="f8b1b-145">В этом разделе показано, как использовать хранилище таблиц Azure или базы данных для хранения сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="f8b1b-146">Этот подход работает при наличии нескольких веб-серверов, так как каждый веб-сервер может взаимодействовать с один и тот же репозиторий данных.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="f8b1b-147">Если остановить веб-серверов, работает или повторного запуска приложения, `OnDisconnected` метод не вызывается.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="f8b1b-148">Таким образом существует возможность, что репозиторий данных будет иметь записи идентификаторов подключений, которые больше не являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="f8b1b-149">Чтобы очистить эти потерянные записи, вы можете сделать недействительным любые соединения, который был создан за пределами периодичностью, необходимые для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="f8b1b-150">В примерах этого раздела включают в себя значение для отслеживания при создании подключения, но не показано, как удалить старые записи, поскольку вы можете сделать это в фоновом процессе.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="f8b1b-151">База данных</span><span class="sxs-lookup"><span data-stu-id="f8b1b-151">Database</span></span>

<span data-ttu-id="f8b1b-152">Следующие примеры показывают, как сохранить сведения о соединении и пользователя в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="f8b1b-153">Можно использовать любые технологии доступа к данным; Тем не менее в приведенном ниже примере показано, как для определения моделей с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="f8b1b-154">Эти модели сущности соответствуют таблиц базы данных и полей.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="f8b1b-155">Структуру данных может значительно изменяться в зависимости от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="f8b1b-156">Первый пример показано, как определить Пользовательская сущность, которая может быть связан с нескольких сущностей соединения.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="f8b1b-157">Затем из вещей, отслеживать состояние каждого подключения с кодом, показанным ниже.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="f8b1b-158">Хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="f8b1b-158">Azure table storage</span></span>

<span data-ttu-id="f8b1b-159">В следующем примере хранилища таблиц Azure как в примере базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="f8b1b-160">Он не включает все сведения, которые необходимо приступить к работе со службой хранилища таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="f8b1b-161">Сведения см. в разделе [Практическое использование табличного хранилища из .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="f8b1b-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="f8b1b-162">В следующем примере показано сущности таблицы для хранения сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="f8b1b-163">Он разбивает данные по имени пользователя и определяет каждую сущность по идентификатор подключения, поэтому пользователь может иметь несколько подключений в любое время.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="f8b1b-164">В концентраторе отслеживать состояние соединения каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
