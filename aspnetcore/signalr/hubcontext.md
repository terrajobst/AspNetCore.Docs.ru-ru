---
title: SignalR HubContext
author: rachelappel
description: Узнайте, как использовать службу ASP.NET Core SignalR HubContext для отправки уведомлений клиентам за пределами концентратора.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726096"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="be6e5-103">Отправка сообщения извне концентратора</span><span class="sxs-lookup"><span data-stu-id="be6e5-103">Send messages from outside a hub</span></span>

<span data-ttu-id="be6e5-104">По [Mengistu Микаэль](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="be6e5-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="be6e5-105">Центр SignalR — это основная абстракция для отправки сообщений для клиентов, подключенных к серверу SignalR.</span><span class="sxs-lookup"><span data-stu-id="be6e5-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="be6e5-106">Можно также отправлять сообщения из других частях приложения с использованием `IHubContext` службы.</span><span class="sxs-lookup"><span data-stu-id="be6e5-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="be6e5-107">В этой статье объясняется, как получить доступ к SignalR `IHubContext` для отправки уведомлений клиентам за пределами концентратора.</span><span class="sxs-lookup"><span data-stu-id="be6e5-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="be6e5-108">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(как для загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="be6e5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="be6e5-109">Получите экземпляр класса `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="be6e5-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="be6e5-110">В ASP.NET Core SignalR, можно обращаться к экземпляру компонента `IHubContext` через внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="be6e5-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="be6e5-111">Можно запустить экземпляр `IHubContext` в контроллере, по промежуточного слоя или другая служба DI.</span><span class="sxs-lookup"><span data-stu-id="be6e5-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="be6e5-112">Используйте экземпляр для отправки сообщений клиентам.</span><span class="sxs-lookup"><span data-stu-id="be6e5-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="be6e5-113">Это отличается от ASP.NET SignalR, которое используется для предоставления доступа к GlobalHost `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="be6e5-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="be6e5-114">ASP.NET Core имеет платформа внедрения зависимостей, которая избавляет от необходимости этот глобальный singleton.</span><span class="sxs-lookup"><span data-stu-id="be6e5-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="be6e5-115">Запустить экземпляр `IHubContext` в контроллере</span><span class="sxs-lookup"><span data-stu-id="be6e5-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="be6e5-116">Можно запустить экземпляр `IHubContext` в контроллер, добавив его в конструктор:</span><span class="sxs-lookup"><span data-stu-id="be6e5-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="be6e5-117">Теперь с доступом к экземпляру `IHubContext`, как будто находятся в самой концентратора можно вызывать методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="be6e5-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="be6e5-118">Получить экземпляр `IHubContext` в по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="be6e5-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="be6e5-119">Доступ `IHubContext` в рамках конвейера по промежуточного слоя следующим образом:</span><span class="sxs-lookup"><span data-stu-id="be6e5-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="be6e5-120">При вызове методов концентратора из вне `Hub` класса, то связанные с вызовом вызывающий объект.</span><span class="sxs-lookup"><span data-stu-id="be6e5-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="be6e5-121">Таким образом, нет нет доступа к `ConnectionId`, `Caller`, и `Others` свойства.</span><span class="sxs-lookup"><span data-stu-id="be6e5-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="be6e5-122">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="be6e5-122">Related resources</span></span>

* [<span data-ttu-id="be6e5-123">Начало работы</span><span class="sxs-lookup"><span data-stu-id="be6e5-123">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="be6e5-124">Центры</span><span class="sxs-lookup"><span data-stu-id="be6e5-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="be6e5-125">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="be6e5-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
