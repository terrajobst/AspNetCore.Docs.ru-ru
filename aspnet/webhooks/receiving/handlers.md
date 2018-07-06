---
uid: webhooks/receiving/handlers
title: Обработчики ASP.NET веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Способ обработки запросов в ASP.NET веб-перехватчики.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: bc8f4ef3f4ade775b395d73dfa8d73fec92fba3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841426"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="0746b-103">Обработчики ASP.NET веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="0746b-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="0746b-104">После проверки веб-перехватчики запросов веб-перехватчика получателем готовых к обработке с помощью пользовательского кода.</span><span class="sxs-lookup"><span data-stu-id="0746b-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="0746b-105">Именно здесь *обработчики* бывают.</span><span class="sxs-lookup"><span data-stu-id="0746b-105">This is where *handlers* come in.</span></span> <span data-ttu-id="0746b-106">Обработчики являются производными от [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) интерфейс, но, как правило, использует [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) класс не непосредственно от интерфейса.</span><span class="sxs-lookup"><span data-stu-id="0746b-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="0746b-107">Запрос объекта WebHook могут обрабатываться один или несколько обработчиков.</span><span class="sxs-lookup"><span data-stu-id="0746b-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="0746b-108">Обработчики вызываются в порядке, основанном на соответствующих им *порядок* переход от наименьшего к наибольшим где порядок является простым целым числом (желательно находиться в диапазоне от 1 до 100) свойства:</span><span class="sxs-lookup"><span data-stu-id="0746b-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Обработчик веб-перехватчика порядок свойства схемы](_static/Handlers.png)

<span data-ttu-id="0746b-110">Обработчик может при необходимости задать *ответа* свойство WebHookHandlerContext, что приведет к обработке stop и ответа для отправки обратно в HTTP-ответа для веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="0746b-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="0746b-111">В приведенном выше случае C обработчика не вызываются, так как она содержит более высокого порядка, чем B, а B задает ответ.</span><span class="sxs-lookup"><span data-stu-id="0746b-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="0746b-112">Настройки ответа используется обычно только веб-перехватчиков где ответ может нести информацию в исходный API.</span><span class="sxs-lookup"><span data-stu-id="0746b-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="0746b-113">Например это происходит с веб-перехватчиками Slack, когда ответ отправляется обратно канал, откуда поступили веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="0746b-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="0746b-114">Обработчики можно задать свойство получателя, если они только хотят получать веб-перехватчики от этого конкретного получателя.</span><span class="sxs-lookup"><span data-stu-id="0746b-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="0746b-115">Если они не заданы получателя они вызываются для всех из них.</span><span class="sxs-lookup"><span data-stu-id="0746b-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="0746b-116">Другие распространенные ответ применяется для использования *410 — потеряно* ответе, чтобы указать, что веб-перехватчика больше не является активным, и отправлять новые запросы.</span><span class="sxs-lookup"><span data-stu-id="0746b-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="0746b-117">По умолчанию обработчик будет вызываться все получатели веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="0746b-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="0746b-118">Тем не менее если *получателя* свойству присваивается имя обработчика, а затем запросы веб-перехватчика этот обработчик получит только от этого получателя.</span><span class="sxs-lookup"><span data-stu-id="0746b-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="0746b-119">Обработка веб-перехватчика</span><span class="sxs-lookup"><span data-stu-id="0746b-119">Processing a WebHook</span></span>

<span data-ttu-id="0746b-120">Когда обработчик вызывается, он получает [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) информацией о запросе веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="0746b-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="0746b-121">Данные, обычно HTTP-запроса, будут доступны из *данных* свойство.</span><span class="sxs-lookup"><span data-stu-id="0746b-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="0746b-122">Тип данных — обычно JSON или HTML-формы данных, но это можно выполнить приведение к более конкретному типу, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="0746b-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="0746b-123">Например, пользовательские веб-перехватчиков, созданных ASP.NET веб-перехватчики может быть приведен к типу [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0746b-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="0746b-124">В очереди обработки</span><span class="sxs-lookup"><span data-stu-id="0746b-124">Queued Processing</span></span>

<span data-ttu-id="0746b-125">Большинство веб-перехватчика отправителей перешлет веб-перехватчика, если ответ не создаются в несколько секунд.</span><span class="sxs-lookup"><span data-stu-id="0746b-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="0746b-126">Это означает, что ваш обработчик необходимо выполнить обработку в течение этого промежутка времени, не для того чтобы вызываться снова.</span><span class="sxs-lookup"><span data-stu-id="0746b-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="0746b-127">Если обработка занимает больше времени, или лучше обрабатывается отдельно то [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) может использоваться для отправки запроса веб-перехватчика в очередь, например [очереди службы хранилища Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="0746b-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="0746b-128">Структура [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) реализован здесь:</span><span class="sxs-lookup"><span data-stu-id="0746b-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
