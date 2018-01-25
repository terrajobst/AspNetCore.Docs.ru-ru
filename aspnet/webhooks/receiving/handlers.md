---
uid: webhooks/receiving/handlers
title: "Обработчики ASP.NET веб-перехватчиков | Документы Microsoft"
author: rick-anderson
description: "Способ обработки запросов в ASP.NET веб-привязок."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a>Обработчики веб-перехватчиков ASP.NET

После проверки веб-перехватчиков запросов веб-перехватчика получатель готов обработать с помощью пользовательского кода. Это место, куда *обработчики* бывают. Обработчики являются производными от [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) интерфейс, но обычно использует [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) класса вместо наследования непосредственно из интерфейса.

Запрос веб-перехватчик может обрабатываться один или несколько обработчиков. Обработчики вызываются в порядке, основанном на соответствующих им *порядок* свойство переходом от меньшего к наибольшим которых порядок является простой целое число со знаком (желательно находиться в диапазоне от 1 до 100):

![Обработчик веб-перехватчика порядок свойства схемы](_static/Handlers.png)

Можно дополнительно задать обработчик *ответ* свойство WebHookHandlerContext, следовательно, обработка остановите и ответа для отправки обратно в ответ HTTP для веб-перехватчик. В приведенных выше примеров из-за более высокого порядка, чем B и B задает ответ не будет вызван обработчик C.

Параметр ответа применяется обычно только для веб-привязок, где ответ может содержать сведения обратно в исходный API. Это пример в случае с веб-перехватчиков Slack, когда ответ отправляется обратно происхождения веб-перехватчик канала. Обработчики можно задать свойство получателя, если только они хотят получать веб-перехватчиков от этого конкретного получателя. Если они не заданы получателю они вызываются для всех из них.

Другие распространенные ответ применяется для использования *410 останова* уведомление о том, что веб-перехватчик больше не является активным и отправлять новые запросы.

По умолчанию все веб-перехватчика получателями будет вызван обработчик. Однако если *получателя* свойству присвоено имя обработчика события, а затем этот обработчик только будет получать запросы веб-перехватчика от этого получателя.

## <a name="processing-a-webhook"></a>Обработка веб-перехватчика

Когда обработчик вызывается, он получает [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) содержащий сведения о веб-перехватчика запроса. Данные, обычно HTTP-запроса, недоступны из *данные* свойство.

Тип данных обычно является JSON или HTML-формы данных, однако можно привести к более конкретному типу, при необходимости. Например, пользовательские веб-перехватчиков, созданные веб-перехватчиков ASP.NET может быть приведен к тип [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) следующим образом:

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

Большинство веб-перехватчика отправителей перешлет веб-перехватчика, если ответ создается только в пределах нескольких секунд. Это означает, что ваш обработчик необходимо завершить обработки за некоторый период времени, чтобы не вызывать ее снова.

Если обработка занимает больше времени, а также лучше обрабатывать отдельно то [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) может использоваться для отправки запроса веб-перехватчик в очередь, например [очередь хранилища Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Контур [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) реализация предоставляется здесь:

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
