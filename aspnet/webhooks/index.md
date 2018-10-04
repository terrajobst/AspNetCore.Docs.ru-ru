---
uid: webhooks/index
title: Общие сведения об ASP.NET для веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Введение в ASP.NET веб-перехватчики.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "48254962"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="5a765-103">Общие сведения о веб-перехватчики ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5a765-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="5a765-104">Веб-перехватчик — это упрощенный HTTP-шаблон, обеспечивающий простую модель подписки для связи друг с другом веб-API и SaaS-служб.</span><span class="sxs-lookup"><span data-stu-id="5a765-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="5a765-105">Когда в службе происходит событие, зарегистрированным подписчикам отправляется уведомление в форме POST HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="5a765-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="5a765-106">Запрос POST содержит сведения о событии, благодаря чему получатель может выполнить соответствующие действия.</span><span class="sxs-lookup"><span data-stu-id="5a765-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="5a765-107">Благодаря своей простоте веб-перехватчики уже доступ к которым предоставляет большое количество служб в том числе [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)и многие другие.</span><span class="sxs-lookup"><span data-stu-id="5a765-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="5a765-108">Например, веб-перехватчика можно указать, что файл был изменен в [Dropbox](http://dropbox.com/), изменения кода будет зафиксирована в GitHub или инициирована платеж в [PayPal](http://www.paypal.com/), или карточки будет создана в [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="5a765-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="5a765-109">Возможности безграничны!</span><span class="sxs-lookup"><span data-stu-id="5a765-109">The possibilities are endless!</span></span>

<span data-ttu-id="5a765-110">Веб-перехватчики Microsoft ASP.NET упрощает процесс отправки и получения веб-перехватчики как часть вашего приложения ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5a765-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="5a765-111">На принимающей стороне он предоставляет общую модель для получения и обработки веб-перехватчиков из произвольного числа поставщиков веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="5a765-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="5a765-112">Он поставляется с поддержкой [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) и [Zendesk](https://www.zendesk.com/) , но легко добавить поддержку для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="5a765-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="5a765-113">На отправляющей стороне он обеспечивает поддержку управления и хранения подписки также, как и для отправки уведомлений о событиях для правильного набора подписчиков.</span><span class="sxs-lookup"><span data-stu-id="5a765-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="5a765-114">Это позволяет определить собственный набор событий, что подписчики может подписаться на и уведомляйте их, когда происходит вещи.</span><span class="sxs-lookup"><span data-stu-id="5a765-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="5a765-115">Эти две части можно использовать друг с другом или друг от друга в зависимости от сценария.</span><span class="sxs-lookup"><span data-stu-id="5a765-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="5a765-116">Если необходимо получать веб-перехватчики от других служб, то можно использовать только ту ее часть получателя; требуется только для предоставления веб-перехватчики для других пользователей для использования, то есть именно это.</span><span class="sxs-lookup"><span data-stu-id="5a765-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="5a765-117">Код предназначен для веб-API 2 ASP.NET и ASP.NET MVC 5 и доступен в виде [открытым исходным кодом на GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="5a765-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="5a765-118">Общие сведения о веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="5a765-118">WebHooks Overview</span></span>

<span data-ttu-id="5a765-119">Веб-перехватчики — это шаблон, который означает, что он зависит, как она используется из службы к службе, но базовый принцип остается тем же.</span><span class="sxs-lookup"><span data-stu-id="5a765-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="5a765-120">Можно считать веб-перехватчиков модель простого pub/sub где пользователь может подписаться на события, происходящие в другом месте.</span><span class="sxs-lookup"><span data-stu-id="5a765-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="5a765-121">Уведомления о событиях, распространяются в виде запросов HTTP POST, содержащий сведения о самом событии.</span><span class="sxs-lookup"><span data-stu-id="5a765-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="5a765-122">Обычно запрос HTTP POST содержит объект JSON или данные HTML-формы, определенный отправителем веб-перехватчика, включая сведения о событии, вызывает веб-перехватчика для активации.</span><span class="sxs-lookup"><span data-stu-id="5a765-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="5a765-123">Например, пример тела запроса POST веб-перехватчика из [GitHub](http://www.github.com/) выглядит следующим образом в результате новая проблема открывается в конкретном репозитории:</span><span class="sxs-lookup"><span data-stu-id="5a765-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="5a765-124">Чтобы действительно является веб-перехватчика от предполагаемого отправителя, запрос POST защищена иным образом и затем проверяется до получателя.</span><span class="sxs-lookup"><span data-stu-id="5a765-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="5a765-125">Например [веб-перехватчики GitHub](https://developer.github.com/webhooks/) включает в себя *X-Hub-подпись* заголовок HTTP с хэшем в тексте запроса, которое проверяется с помощью реализации получателя, не нужно беспокоиться об этом.</span><span class="sxs-lookup"><span data-stu-id="5a765-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="5a765-126">Как правило, веб-перехватчика поток выглядит примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5a765-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="5a765-127">Веб-перехватчика Отправитель создает события, которые клиент может подписаться.</span><span class="sxs-lookup"><span data-stu-id="5a765-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="5a765-128">События описывают заметные изменения в систему, например, элемент данных было вставлено, что процесс завершен, или что-то другое.</span><span class="sxs-lookup"><span data-stu-id="5a765-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="5a765-129">Получатель веб-перехватчика подписывается путем регистрации веб-перехватчика, состоящий из четырех возможных:</span><span class="sxs-lookup"><span data-stu-id="5a765-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="5a765-130">URI, для которой уведомление о событии должны быть опубликованы в виде запроса HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="5a765-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="5a765-131">Набор фильтров, описывающий определенного события, для которых должны создаваться веб-перехватчика;</span><span class="sxs-lookup"><span data-stu-id="5a765-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="5a765-132">Секретный ключ, который используется для подписи запроса HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="5a765-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="5a765-133">Дополнительные данные, который следует включить в запрос HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5a765-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="5a765-134">Например, это может быть дополнительные поля заголовка HTTP или свойств, включенных в тексте запроса HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5a765-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="5a765-135">Когда происходит событие, можно найти в соответствующей регистрации веб-перехватчика и отправляет запросы HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5a765-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="5a765-136">Как правило создания запросов HTTP POST повторяются несколько раз, если для какой-либо причине получателя не отвечает или результаты запроса HTTP POST в ответ на ошибку.</span><span class="sxs-lookup"><span data-stu-id="5a765-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="5a765-137">Конвейер обработки веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="5a765-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="5a765-138">Конвейер обработки веб-перехватчики Microsoft ASP.NET для входящего веб-перехватчиков выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5a765-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Конвейер обработки ASP.NET веб-перехватчиков](_static/WebHookReceivers.png)

<span data-ttu-id="5a765-140">Двумя ключевыми понятиями являются *получателей* и *обработчики*:</span><span class="sxs-lookup"><span data-stu-id="5a765-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="5a765-141">*Приемники* отвечают за обработку определенного вида веб-перехватчика из заданного отправителя, а также для принудительного применения проверки безопасности, чтобы убедиться, что предполагаемым отправитель действительно является запроса веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="5a765-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="5a765-142">*Обработчики* обычно являются, где код пользователя запускают обработку определенного веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="5a765-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="5a765-143">В следующие узлы эти понятия описаны более подробно.</span><span class="sxs-lookup"><span data-stu-id="5a765-143">In the following nodes these concepts are described in more details.</span></span>
