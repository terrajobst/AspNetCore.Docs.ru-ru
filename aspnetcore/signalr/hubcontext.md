---
title: SignalR HubContext
author: tdykstra
description: Узнайте, как использовать службу ASP.NET Core SignalR HubContext для отправки уведомлений клиентам из за пределами концентратору.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: bb07a3b5c6e153092635fa4e1283619777865a53
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325358"
---
# <a name="send-messages-from-outside-a-hub"></a>Отправка сообщения извне концентратору

По [Майкл Mengistu](https://twitter.com/MikaelM_12)

Центр SignalR — это основная абстракция для отправки сообщений для клиентов, подключенных к серверу SignalR. Можно также отправлять сообщения из других мест в приложении с помощью `IHubContext` службы. В этой статье объясняется, как получить доступ к SignalR `IHubContext` для отправки уведомлений клиентам из за пределами концентратору.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)

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

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
