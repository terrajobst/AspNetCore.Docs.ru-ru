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
# <a name="send-messages-from-outside-a-hub"></a>Отправка сообщения извне концентратору

По [Майкл Mengistu](https://twitter.com/MikaelM_12)

Центр SignalR — это основная абстракция для отправки сообщений для клиентов, подключенных к серверу SignalR. Можно также отправлять сообщения из других мест в приложении с помощью `IHubContext` службы. В этой статье объясняется, как получить доступ к SignalR `IHubContext` для отправки уведомлений клиентам из за пределами концентратору.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Получить экземпляр IHubContext

В ASP.NET Core SignalR, можно получить доступ к экземпляру `IHubContext` посредством внедрения зависимостей. Вы можете внедрить экземпляр `IHubContext` в контроллере, по промежуточного слоя или другую службу внедрения Зависимостей. Используйте экземпляр, для отправки сообщений клиентам.

> [!NOTE]
> Это отличается от ASP.NET 4.x SignalR, который используется для предоставления доступа к GlobalHost `IHubContext`. ASP.NET Core имеет платформой внедрения зависимостей, который устраняет потребность в этот глобальный одноэлементный.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Экземпляр IHubContext в контроллере

Вы можете внедрить экземпляр `IHubContext` в контроллер, добавьте его в ваш конструктор:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Теперь, с доступом к экземпляру `IHubContext`, можно вызывать методы концентратора, как если бы вы были в сам концентратор.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Получить экземпляр IHubContext в по промежуточного слоя

Доступ `IHubContext` в конвейер по промежуточного слоя следующим образом:

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> При вызове методов концентратора из за пределами `Hub` класса, то связанные с вызовом вызывающий объект. Таким образом, отсутствует доступ к `ConnectionId`, `Caller`, и `Others` свойства.

### <a name="inject-a-strongly-typed-hubcontext"></a>Внедрить HubContext со строгой типизацией

Для вставки HubContext со строгой типизацией, убедитесь, концентратор наследует от `Hub<T>`. Внедрить его с помощью `IHubContext<THub, T>` интерфейс вместо `IHubContext<THub>`.

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

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
