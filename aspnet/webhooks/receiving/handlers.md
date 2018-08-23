---
uid: webhooks/receiving/handlers
title: Обработчики ASP.NET веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Способ обработки запросов в ASP.NET веб-перехватчики.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836274"
---
# <a name="aspnet-webhooks-handlers"></a>Обработчики ASP.NET веб-перехватчиков

После проверки веб-перехватчики запросов веб-перехватчика получателем готовых к обработке с помощью пользовательского кода. Именно здесь *обработчики* бывают. Обработчики являются производными от [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) интерфейс, но, как правило, использует [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) класс не непосредственно от интерфейса.

Запрос объекта WebHook могут обрабатываться один или несколько обработчиков. Обработчики вызываются в порядке, основанном на соответствующих им *порядок* переход от наименьшего к наибольшим где порядок является простым целым числом (желательно находиться в диапазоне от 1 до 100) свойства:

![Обработчик веб-перехватчика порядок свойства схемы](_static/Handlers.png)

Обработчик может при необходимости задать *ответа* свойство WebHookHandlerContext, что приведет к обработке stop и ответа для отправки обратно в HTTP-ответа для веб-перехватчика. В приведенном выше случае C обработчика не вызываются, так как она содержит более высокого порядка, чем B, а B задает ответ.

Настройки ответа используется обычно только веб-перехватчиков где ответ может нести информацию в исходный API. Например это происходит с веб-перехватчиками Slack, когда ответ отправляется обратно канал, откуда поступили веб-перехватчика. Обработчики можно задать свойство получателя, если они только хотят получать веб-перехватчики от этого конкретного получателя. Если они не заданы получателя они вызываются для всех из них.

Другие распространенные ответ применяется для использования *410 — потеряно* ответе, чтобы указать, что веб-перехватчика больше не является активным, и отправлять новые запросы.

По умолчанию обработчик будет вызываться все получатели веб-перехватчика. Тем не менее если *получателя* свойству присваивается имя обработчика, а затем запросы веб-перехватчика этот обработчик получит только от этого получателя.

## <a name="processing-a-webhook"></a>Обработка веб-перехватчика

Когда обработчик вызывается, он получает [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) информацией о запросе веб-перехватчика. Данные, обычно HTTP-запроса, будут доступны из *данных* свойство.

Тип данных — обычно JSON или HTML-формы данных, но это можно выполнить приведение к более конкретному типу, при необходимости. Например, пользовательские веб-перехватчиков, созданных ASP.NET веб-перехватчики может быть приведен к типу [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) следующим образом:

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>В очереди обработки

Большинство веб-перехватчика отправителей перешлет веб-перехватчика, если ответ не создаются в несколько секунд. Это означает, что ваш обработчик необходимо выполнить обработку в течение этого промежутка времени, не для того чтобы вызываться снова.

Если обработка занимает больше времени, или лучше обрабатывается отдельно то [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) может использоваться для отправки запроса веб-перехватчика в очередь, например [очереди службы хранилища Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Структура [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) реализован здесь:

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
