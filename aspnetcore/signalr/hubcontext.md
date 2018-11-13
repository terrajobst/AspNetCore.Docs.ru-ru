---
title: SignalR HubContext
author: tdykstra
description: Узнайте, как использовать службу ASP.NET Core SignalR HubContext для отправки уведомлений клиентам из за пределами концентратору.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: ce350147e743df7f1671dd86da7c83f04bf0fe22
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569938"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="67f02-103">Отправка сообщения извне концентратору</span><span class="sxs-lookup"><span data-stu-id="67f02-103">Send messages from outside a hub</span></span>

<span data-ttu-id="67f02-104">По [Майкл Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="67f02-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="67f02-105">Центр SignalR — это основная абстракция для отправки сообщений для клиентов, подключенных к серверу SignalR.</span><span class="sxs-lookup"><span data-stu-id="67f02-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="67f02-106">Можно также отправлять сообщения из других мест в приложении с помощью `IHubContext` службы.</span><span class="sxs-lookup"><span data-stu-id="67f02-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="67f02-107">В этой статье объясняется, как получить доступ к SignalR `IHubContext` для отправки уведомлений клиентам из за пределами концентратору.</span><span class="sxs-lookup"><span data-stu-id="67f02-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="67f02-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="67f02-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="67f02-109">Получить экземпляр IHubContext</span><span class="sxs-lookup"><span data-stu-id="67f02-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="67f02-110">В ASP.NET Core SignalR, можно получить доступ к экземпляру `IHubContext` посредством внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="67f02-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="67f02-111">Вы можете внедрить экземпляр `IHubContext` в контроллере, по промежуточного слоя или другую службу внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="67f02-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="67f02-112">Используйте экземпляр, для отправки сообщений клиентам.</span><span class="sxs-lookup"><span data-stu-id="67f02-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="67f02-113">Это отличается от ASP.NET 4.x SignalR, который используется для предоставления доступа к GlobalHost `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="67f02-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="67f02-114">ASP.NET Core имеет платформой внедрения зависимостей, который устраняет потребность в этот глобальный одноэлементный.</span><span class="sxs-lookup"><span data-stu-id="67f02-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="67f02-115">Экземпляр IHubContext в контроллере</span><span class="sxs-lookup"><span data-stu-id="67f02-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="67f02-116">Вы можете внедрить экземпляр `IHubContext` в контроллер, добавьте его в ваш конструктор:</span><span class="sxs-lookup"><span data-stu-id="67f02-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="67f02-117">Теперь, с доступом к экземпляру `IHubContext`, можно вызывать методы концентратора, как если бы вы были в сам концентратор.</span><span class="sxs-lookup"><span data-stu-id="67f02-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="67f02-118">Получить экземпляр IHubContext в по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="67f02-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="67f02-119">Доступ `IHubContext` в конвейер по промежуточного слоя следующим образом:</span><span class="sxs-lookup"><span data-stu-id="67f02-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="67f02-120">При вызове методов концентратора из за пределами `Hub` класса, то связанные с вызовом вызывающий объект.</span><span class="sxs-lookup"><span data-stu-id="67f02-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="67f02-121">Таким образом, отсутствует доступ к `ConnectionId`, `Caller`, и `Others` свойства.</span><span class="sxs-lookup"><span data-stu-id="67f02-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

### <a name="inject-a-strongly-typed-hubcontext"></a><span data-ttu-id="67f02-122">Внедрить HubContext со строгой типизацией</span><span class="sxs-lookup"><span data-stu-id="67f02-122">Inject a strongly-typed HubContext</span></span>

<span data-ttu-id="67f02-123">Для вставки HubContext со строгой типизацией, убедитесь, концентратор наследует от `Hub<T>`.</span><span class="sxs-lookup"><span data-stu-id="67f02-123">To inject a strongly-typed HubContext, ensure your Hub inherits from `Hub<T>`.</span></span> <span data-ttu-id="67f02-124">Внедрить его с помощью `IHubContext<THub, T>` интерфейс вместо `IHubContext<THub>`.</span><span class="sxs-lookup"><span data-stu-id="67f02-124">Inject it using the `IHubContext<THub, T>` interface rather than `IHubContext<THub>`.</span></span>

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a><span data-ttu-id="67f02-125">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="67f02-125">Related resources</span></span>

* [<span data-ttu-id="67f02-126">Начало работы</span><span class="sxs-lookup"><span data-stu-id="67f02-126">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="67f02-127">Центры</span><span class="sxs-lookup"><span data-stu-id="67f02-127">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="67f02-128">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="67f02-128">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
