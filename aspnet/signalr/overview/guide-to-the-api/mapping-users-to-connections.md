---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "Сопоставление пользователей SignalR с подключениями | Документы Microsoft"
author: tfitzmac
description: "В этом разделе показано, как сохранить сведения о пользователях и их соединения. Патрик Флетчера помогли записи в этом разделе. Версии программного обеспечения, используемого в этом разделе..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9b50d8805beabbc48467e20331c7593de9bc4254
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="6e528-105">Сопоставление пользователей SignalR для подключения</span><span class="sxs-lookup"><span data-stu-id="6e528-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="6e528-106">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6e528-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6e528-107">В этом разделе показано, как сохранить сведения о пользователях и их соединения.</span><span class="sxs-lookup"><span data-stu-id="6e528-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="6e528-108">Патрик Флетчера помогли записи в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6e528-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6e528-109">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="6e528-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6e528-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6e528-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6e528-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6e528-111">.NET 4.5</span></span>
> - <span data-ttu-id="6e528-112">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="6e528-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6e528-113">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="6e528-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="6e528-114">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6e528-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6e528-115">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="6e528-115">Questions and comments</span></span>
> 
> <span data-ttu-id="6e528-116">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="6e528-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6e528-117">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6e528-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="6e528-118">Вступление</span><span class="sxs-lookup"><span data-stu-id="6e528-118">Introduction</span></span>

<span data-ttu-id="6e528-119">Каждый клиент, подключающийся к концентратору передает уникальный идентификатор соединения. Можно получить это значение в `Context.ConnectionId` свойства контекста концентратора.</span><span class="sxs-lookup"><span data-stu-id="6e528-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="6e528-120">Если приложению необходимо сопоставить пользователя с идентификатором соединения и сохранить это сопоставление, можно использовать один из следующих:</span><span class="sxs-lookup"><span data-stu-id="6e528-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="6e528-121">Поставщик идентификатора пользователя (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="6e528-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="6e528-122">[Хранение в памяти](#inmemory), такие как словарь</span><span class="sxs-lookup"><span data-stu-id="6e528-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="6e528-123">Группа SignalR для каждого пользователя</span><span class="sxs-lookup"><span data-stu-id="6e528-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="6e528-124">[Постоянная, внешнего хранилища](#database), таких как базы данных или хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="6e528-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="6e528-125">В этом разделе показана каждая из этих реализаций.</span><span class="sxs-lookup"><span data-stu-id="6e528-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="6e528-126">Вы используете `OnConnected`, `OnDisconnected`, и `OnReconnected` методы `Hub` класса для отслеживания состояния подключения пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="6e528-127">Зависит от оптимального подхода для приложения.</span><span class="sxs-lookup"><span data-stu-id="6e528-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="6e528-128">Число веб-серверов размещается приложение.</span><span class="sxs-lookup"><span data-stu-id="6e528-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="6e528-129">Нужно ли получить список подключенных пользователей.</span><span class="sxs-lookup"><span data-stu-id="6e528-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="6e528-130">Следует ли сохранять сведения пользователей и групп, когда приложение или сервер перезагружается.</span><span class="sxs-lookup"><span data-stu-id="6e528-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="6e528-131">Задержка при вызове внешнего сервера является ли проблема.</span><span class="sxs-lookup"><span data-stu-id="6e528-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="6e528-132">В следующей таблице показано, какой подход работает для этих рекомендациях.</span><span class="sxs-lookup"><span data-stu-id="6e528-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="6e528-133">Более одного сервера</span><span class="sxs-lookup"><span data-stu-id="6e528-133">More than one server</span></span> | <span data-ttu-id="6e528-134">Получить список подключенных пользователей</span><span class="sxs-lookup"><span data-stu-id="6e528-134">Get list of currently connected users</span></span> | <span data-ttu-id="6e528-135">Сохранять данные после перезагрузки</span><span class="sxs-lookup"><span data-stu-id="6e528-135">Persist information after restarts</span></span> | <span data-ttu-id="6e528-136">Оптимальной производительности</span><span class="sxs-lookup"><span data-stu-id="6e528-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="6e528-137">ИД пользователя поставщика</span><span class="sxs-lookup"><span data-stu-id="6e528-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="6e528-138">In-memory</span><span class="sxs-lookup"><span data-stu-id="6e528-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="6e528-139">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="6e528-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="6e528-140">Постоянными, внешний</span><span class="sxs-lookup"><span data-stu-id="6e528-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="6e528-141">Поставщик IUserID</span><span class="sxs-lookup"><span data-stu-id="6e528-141">IUserID provider</span></span>

<span data-ttu-id="6e528-142">Эта функция позволяет пользователям указать идентификатор пользователя — в зависимости от IRequest через новый интерфейс IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="6e528-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="6e528-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="6e528-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="6e528-144">По умолчанию, будет реализация, которая использует пользователя `IPrincipal.Identity.Name` имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="6e528-145">Чтобы изменить этот параметр, зарегистрируйте реализации `IUserIdProvider` с глобальных узлов, при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="6e528-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="6e528-146">Из в концентраторе, вы сможете отправлять сообщения пользователям через следующие API:</span><span class="sxs-lookup"><span data-stu-id="6e528-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="6e528-147">**Отправка сообщения для определенного пользователя**</span><span class="sxs-lookup"><span data-stu-id="6e528-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="6e528-148">Хранение в памяти</span><span class="sxs-lookup"><span data-stu-id="6e528-148">In-memory storage</span></span>

<span data-ttu-id="6e528-149">Следующие примеры показывают, как сохранить сведения о соединении и пользователя в словарь, который хранится в памяти.</span><span class="sxs-lookup"><span data-stu-id="6e528-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="6e528-150">Словарь использует `HashSet` для хранения идентификатора соединения. В любой момент пользователь может иметь несколько подключений к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e528-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="6e528-151">Например пользователь, который подключается с помощью нескольких устройств или несколько вкладок браузер будет иметь более одного идентификатор подключения.</span><span class="sxs-lookup"><span data-stu-id="6e528-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="6e528-152">Если приложение завершает работу, все данные теряются, но он будет повторно заполнен как пользователи повторно устанавливать их соединения.</span><span class="sxs-lookup"><span data-stu-id="6e528-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="6e528-153">Хранение в памяти не работает, если в среде имеются несколько веб-сервера, поскольку каждый сервер в отдельную коллекцию соединений.</span><span class="sxs-lookup"><span data-stu-id="6e528-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="6e528-154">В первом примере класс, который управляет сопоставления пользователей для подключения.</span><span class="sxs-lookup"><span data-stu-id="6e528-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="6e528-155">Ключ для HashSet будет имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="6e528-156">Следующий пример показано, как использовать класс сопоставления подключения от концентратора.</span><span class="sxs-lookup"><span data-stu-id="6e528-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="6e528-157">Экземпляр класса хранится в имени переменной `_connections`.</span><span class="sxs-lookup"><span data-stu-id="6e528-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="6e528-158">Группы в однопользовательском режиме</span><span class="sxs-lookup"><span data-stu-id="6e528-158">Single-user groups</span></span>

<span data-ttu-id="6e528-159">Можно создать группу для каждого пользователя и отправить сообщение в эту группу для достижения только данный пользователь.</span><span class="sxs-lookup"><span data-stu-id="6e528-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="6e528-160">Имя каждой группы является имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="6e528-161">Если пользователь имеет более одного соединения, каждый идентификатор соединения добавляется к группе пользователей.</span><span class="sxs-lookup"><span data-stu-id="6e528-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="6e528-162">Не следует удалять вручную пользователя из группы при отключении пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="6e528-163">Это действие выполняется автоматически платформой SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e528-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="6e528-164">Приведенный ниже показано, как реализовать группы одного пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="6e528-165">Постоянные, внешние хранилища</span><span class="sxs-lookup"><span data-stu-id="6e528-165">Permanent, external storage</span></span>

<span data-ttu-id="6e528-166">В этом разделе показано, как использовать базы данных или хранилище таблиц Azure для хранения сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="6e528-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="6e528-167">Этот способ подходит при наличии нескольких веб-серверов, так как каждый веб-сервер может взаимодействовать с одного репозитория данных.</span><span class="sxs-lookup"><span data-stu-id="6e528-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="6e528-168">Если остановить веб-серверов, работает или повторного запуска приложения `OnDisconnected` метод не вызывается.</span><span class="sxs-lookup"><span data-stu-id="6e528-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="6e528-169">Таким образом возможно, репозиторий данных будет записи идентификаторов подключений, которые больше не являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="6e528-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="6e528-170">Чтобы удалить эти потерянные записи, вы можете сделать недействительной любые соединения, который был создан за пределами периодичностью, необходимые для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="6e528-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="6e528-171">Примеры в этом разделе содержат значение для отслеживания, когда подключение было создано, но не показано, как удалить старые записи, поскольку вы можете сделать это в фоновом процессе.</span><span class="sxs-lookup"><span data-stu-id="6e528-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="6e528-172">База данных</span><span class="sxs-lookup"><span data-stu-id="6e528-172">Database</span></span>

<span data-ttu-id="6e528-173">Следующие примеры показывают, как сохранить сведения о соединении и пользователя в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6e528-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="6e528-174">Можно использовать любой технологии доступа к данным; Тем не менее в приведенном ниже примере показан способ определения модели с использованием платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6e528-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="6e528-175">Эти модели сущностей соответствующие таблицы базы данных и полей.</span><span class="sxs-lookup"><span data-stu-id="6e528-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="6e528-176">Структуру данных может значительно различаться в зависимости от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="6e528-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="6e528-177">В первом примере показан способ определения Пользовательская сущность, которая может быть связан с большого количества объектов подключения.</span><span class="sxs-lookup"><span data-stu-id="6e528-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="6e528-178">Затем в центре можно отслеживать состояние каждого соединения с приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="6e528-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="6e528-179">Хранилище таблиц Azure</span><span class="sxs-lookup"><span data-stu-id="6e528-179">Azure table storage</span></span>

<span data-ttu-id="6e528-180">В следующем примере хранилища Azure таблицы как в примере базы данных.</span><span class="sxs-lookup"><span data-stu-id="6e528-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="6e528-181">Он не включает все данные, что необходимо начать работу со службой хранилища Azure таблицы.</span><span class="sxs-lookup"><span data-stu-id="6e528-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="6e528-182">Сведения см. в разделе [как использовать хранилище таблиц из .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="6e528-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="6e528-183">В следующем примере показано сущности таблицы для хранения сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="6e528-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="6e528-184">Секционирование данных по имени пользователя и идентифицирует каждую сущность, идентификатор подключения, пользователь может иметь несколько подключений в любое время.</span><span class="sxs-lookup"><span data-stu-id="6e528-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="6e528-185">В концентраторе отслеживать состояние соединения каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="6e528-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
