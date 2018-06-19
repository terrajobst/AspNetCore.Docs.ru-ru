---
uid: webhooks/receiving/handlers
title: Обработчики ASP.NET веб-перехватчиков | Документы Microsoft
author: rick-anderson
description: Способ обработки запросов в ASP.NET веб-привязок.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883675"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="04d2b-103">Обработчики веб-перехватчиков ASP.NET</span><span class="sxs-lookup"><span data-stu-id="04d2b-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="04d2b-104">После проверки веб-перехватчиков запросов веб-перехватчика получатель готов обработать с помощью пользовательского кода.</span><span class="sxs-lookup"><span data-stu-id="04d2b-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="04d2b-105">Это место, куда *обработчики* бывают.</span><span class="sxs-lookup"><span data-stu-id="04d2b-105">This is where *handlers* come in.</span></span> <span data-ttu-id="04d2b-106">Обработчики являются производными от [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) интерфейс, но обычно использует [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) класса вместо наследования непосредственно из интерфейса.</span><span class="sxs-lookup"><span data-stu-id="04d2b-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="04d2b-107">Запрос веб-перехватчик может обрабатываться один или несколько обработчиков.</span><span class="sxs-lookup"><span data-stu-id="04d2b-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="04d2b-108">Обработчики вызываются в порядке, основанном на соответствующих им *порядок* свойство переходом от меньшего к наибольшим которых порядок является простой целое число со знаком (желательно находиться в диапазоне от 1 до 100):</span><span class="sxs-lookup"><span data-stu-id="04d2b-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Обработчик веб-перехватчика порядок свойства схемы](_static/Handlers.png)

<span data-ttu-id="04d2b-110">Можно дополнительно задать обработчик *ответ* свойство WebHookHandlerContext, следовательно, обработка остановите и ответа для отправки обратно в ответ HTTP для веб-перехватчик.</span><span class="sxs-lookup"><span data-stu-id="04d2b-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="04d2b-111">В приведенных выше примеров из-за более высокого порядка, чем B и B задает ответ не будет вызван обработчик C.</span><span class="sxs-lookup"><span data-stu-id="04d2b-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="04d2b-112">Параметр ответа применяется обычно только для веб-привязок, где ответ может содержать сведения обратно в исходный API.</span><span class="sxs-lookup"><span data-stu-id="04d2b-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="04d2b-113">Это пример в случае с веб-перехватчиков Slack, когда ответ отправляется обратно происхождения веб-перехватчик канала.</span><span class="sxs-lookup"><span data-stu-id="04d2b-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="04d2b-114">Обработчики можно задать свойство получателя, если только они хотят получать веб-перехватчиков от этого конкретного получателя.</span><span class="sxs-lookup"><span data-stu-id="04d2b-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="04d2b-115">Если они не заданы получателю они вызываются для всех из них.</span><span class="sxs-lookup"><span data-stu-id="04d2b-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="04d2b-116">Другие распространенные ответ применяется для использования *410 останова* уведомление о том, что веб-перехватчик больше не является активным и отправлять новые запросы.</span><span class="sxs-lookup"><span data-stu-id="04d2b-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="04d2b-117">По умолчанию все веб-перехватчика получателями будет вызван обработчик.</span><span class="sxs-lookup"><span data-stu-id="04d2b-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="04d2b-118">Однако если *получателя* свойству присвоено имя обработчика события, а затем этот обработчик только будет получать запросы веб-перехватчика от этого получателя.</span><span class="sxs-lookup"><span data-stu-id="04d2b-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="04d2b-119">Обработка веб-перехватчика</span><span class="sxs-lookup"><span data-stu-id="04d2b-119">Processing a WebHook</span></span>

<span data-ttu-id="04d2b-120">Когда обработчик вызывается, он получает [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) содержащий сведения о веб-перехватчика запроса.</span><span class="sxs-lookup"><span data-stu-id="04d2b-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="04d2b-121">Данные, обычно HTTP-запроса, недоступны из *данные* свойство.</span><span class="sxs-lookup"><span data-stu-id="04d2b-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="04d2b-122">Тип данных обычно является JSON или HTML-формы данных, однако можно привести к более конкретному типу, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="04d2b-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="04d2b-123">Например, пользовательские веб-перехватчиков, созданные веб-перехватчиков ASP.NET может быть приведен к тип [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) следующим образом:</span><span class="sxs-lookup"><span data-stu-id="04d2b-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="04d2b-124">В очереди обработки</span><span class="sxs-lookup"><span data-stu-id="04d2b-124">Queued Processing</span></span>

<span data-ttu-id="04d2b-125">Большинство веб-перехватчика отправителей перешлет веб-перехватчика, если ответ создается только в пределах нескольких секунд.</span><span class="sxs-lookup"><span data-stu-id="04d2b-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="04d2b-126">Это означает, что ваш обработчик необходимо завершить обработки за некоторый период времени, чтобы не вызывать ее снова.</span><span class="sxs-lookup"><span data-stu-id="04d2b-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="04d2b-127">Если обработка занимает больше времени, а также лучше обрабатывать отдельно то [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) может использоваться для отправки запроса веб-перехватчик в очередь, например [очередь хранилища Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="04d2b-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="04d2b-128">Контур [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) реализация предоставляется здесь:</span><span class="sxs-lookup"><span data-stu-id="04d2b-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
